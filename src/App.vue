<template>
  <div id="app">
    <div class="pairs__container">
      <div class="pairs" v-for="(individual, index) in this.allSantasArr" v-bind:key="index">
        <span class="santa">{{individual.first}} {{individual.last}}</span>
        <span class="selection">{{allSantasObj[individual.selection]}}</span>
      </div>
    </div>
    <br />
    <div class="results">
      <div class="pairs--lm" v-for="(individual, index) in this.allSantasArr" v-bind:key="index">
        <span class="santa--lm">{{individual.first}} {{individual.last}}</span>
        <span>----></span>
        <span class="selection--lm">{{allSantasObj[individual.selection]}}</span>
      </div>
    </div>
  </div>
</template>

<script>
import everyoneObj from './people.js'
export default {
  name: 'app',
  components: {},
  methods: {
    createParticipants(allObj) {
      const allParticipantsArray = []
      for (const family in allObj) {
        allObj[family].forEach(personObj => {
          if (personObj.participating) {
            this.allSantasObj[
              personObj.id
            ] = `${personObj.first} ${personObj.last}`
            this.allSantasArr.push(personObj)
            allParticipantsArray.push(personObj.id)
          }
        })
      }
      this.allParticipantIds = allParticipantsArray
    },
    assignSanta(santa, ids, tempIdArr = []) {
      if (santa.participating) {
        const randomId = Math.floor(Math.random() * ids.length)
        // console.log(
        //   'rand num is',
        //   randomId,
        //   '. Assigning ',
        //   santa.first,
        //   santa.last,
        //   ' to id ',
        //   ids[randomId]
        // )
        if (
          santa.directFamIdsArray.includes(ids[randomId]) ||
          santa.previousSelectionIds.includes(ids[randomId])
        ) {
          console.log('AHHHH THIS IS NO GOOD!!', santa.first, ids[randomId])
          this.resetCounter--
          tempIdArr.push(ids[randomId])
          ids.splice(randomId, 1)
          this.assignSanta(santa, ids, tempIdArr)
          if (this.resetCounter < 0 || ids.length === 0) {
            console.log('NOOOOOOOO')
            throw new Error('Something went badly wrong!')
          }
        } else {
          santa.selection = ids[randomId]
          this.resetCounter = 10
          ids = [...ids, ...tempIdArr]
          ids.splice(randomId, 1)
          this.allParticipantIds = ids
        }
      }
      // eslint-disable-next-line
      // console.log('THE SANTAS', this.allParticipantIds)
    },
    distributeallSantasArr() {
      this.allSantasArr.forEach(santa => {
        this.assignSanta(santa, this.allParticipantIds)
      })
    },
    showParticipants() {
      // eslint-disable-next-line
      console.log('santas', this.allSantasArr)
      // eslint-disable-next-line
      console.log('participants', this.allParticipantIds)
    },
    test(selectionId) {
      // eslint-disable-next-line
      // console.log('AHHH', selectionId)
      // eslint-disable-next-line
      // console.log('IDIDID', this.allSantasArr[selectionId].id)
      return this.allSantasArr[selectionId]
    }
  },
  created: function() {
    this.createParticipants(everyoneObj)
    this.distributeallSantasArr()
    this.showParticipants()
  },
  data() {
    return {
      allParticipantIds: null,
      allSantasArr: [],
      allSantasObj: {},
      resetCounter: 10
    }
  }
}
</script>

<style>
.pairs {
  font-size: 25px;
  justify-content: center;
  margin: 75px;
  text-transform: uppercase;
  align-items: baseline;
}

.pairs span {
  margin: 0 15px;
  border-style: solid;
  border-width: 1px;
  padding: 5px;
}

.santa {
  color: green;
  border-color: red;
  cursor: pointer;
}

.selection {
  color: red;
  border-color: green;
  /* visibility: hidden; */
  opacity: 0;
  font-size: 50px;
  transition: opacity 0.25s ease-in;
}

.santa:hover + .selection {
  /* visibility: visible; */
  opacity: 1;
  transition: opacity 0.25s ease-in;
}

/* LM VIEW */

.results {
  margin-top: 750px;
  font-weight: bold;
}

.pairs--lm {
  font-size: 15px;
  justify-content: center;
  margin: 10px;
  text-transform: uppercase;
  align-items: baseline;
}

.pairs--lm span {
  padding: 5px;
}

.santa--lm {
  color: green;
  border-color: red;
  cursor: pointer;
}

.selection--lm {
  color: red;
  border-color: green;
  transition: opacity 0.25s ease-in;
}
</style>
