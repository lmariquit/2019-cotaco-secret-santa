<template>
  <div id="app">
    <div
      v-for="(individual, index) in this.allSantasArr"
      v-bind:key="index"
    >{{individual.first}} {{individual.last}} --> {{allSantasObj[individual.selection]}}</div>
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
        console.log(
          'rand num is',
          randomId,
          '. Assigning ',
          santa.first,
          santa.last,
          ' to id ',
          ids[randomId]
        )
        if (santa.directFamIdsArray.includes(ids[randomId])) {
          console.log('AHHHH THIS IS NO GOOD!!')
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
      console.log('THE SANTAS', this.allParticipantIds)
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
      console.log('AHHH', selectionId)
      // eslint-disable-next-line
      console.log('IDIDID', this.allSantasArr[selectionId].id)
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
.santa {
  color: red;
}
.selection {
  color: blue;
  background: yellow;
}
</style>
