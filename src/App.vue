<template>
  <div id="app">
    <div v-for="(family, index) in people" v-bind:key="index">
      <button v-on:click="shuffle">BUTTON</button>
      <div v-for="(invididual, i) in family" v-bind:key="i">
        <span>{{ individual.id }}</span>
      </div>
    </div>
  </div>
</template>

<script>
// <div
//   class="santa"
//   v-for="(individual, index) in family"
//   v-bind:key="index"
// >
//   <span> {{ individual.first }} {{ individual.last }} </span>
//   <span> --> </span>
//   <span class="selection">{{ individual.selection }}</span>
// </div>

import everyoneObj from './people.js'
export default {
  name: 'app',
  components: {},
  methods: {
    createParticipants(allObj) {
      // eslint-disable-next-line
      // console.log(everyoneObj)
      const allParticipantsArray = []
      for (const family in allObj) {
        allObj[family].forEach(personObj => {
          if (personObj.participating) {
            this.allSantas.push(personObj)
            allParticipantsArray.push(personObj.id)
          }
        })
      }
      this.allParticipantIds = allParticipantsArray
      // eslint-disable-next-line
      // console.log(this.allParticipantIds)
    },
    assignSanta(santa) {
      if (santa.participating) {
        const randomId = Math.floor(
          Math.random() * this.allParticipantIds.length
        )
        console.log(
          'rand num is',
          randomId,
          '. Assigning ',
          santa.first,
          santa.last,
          ' to id ',
          this.allParticipantIds[randomId]
        )
        if (
          santa.directFamIdsArray.includes(this.allParticipantIds[randomId])
        ) {
          console.log('AHHHH THIS IS NO GOOD!!')
          this.resetCounter--
          this.tempIdArr.push(this.allParticipantIds[randomId])
          this.allParticipantIds.splice(randomId, 1)
          if (this.resetCounter < 0) {
            console.log('NOOOOOOOO')
            throw new Error('Something went badly wrong!')
          }
        }
        santa.selection = this.allParticipantIds[randomId]
        this.resetCounter = 10
        this.allParticipantIds = [...this.allParticipantIds, ...this.tempIdArr]
        this.tempIdArr = []
        this.allParticipantIds.splice(randomId, 1)
      }
    },
    distributeAllSantas() {
      this.allSantas.forEach(santa => {
        this.assignSanta(santa)
      })
    },
    showParticipants() {
      // eslint-disable-next-line
      console.log('santas', this.allSantas)
      // eslint-disable-next-line
      console.log('participants', this.allParticipantIds)
    }
  },
  created: function() {
    this.createParticipants(everyoneObj)
    this.distributeAllSantas()
    this.showParticipants()
  },
  data() {
    return {
      allParticipantIds: null,
      allSantas: [],
      resetCounter: 10,
      tempIdArr: []
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
