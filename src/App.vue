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
        if (
          !santa.directFamIdsArray.includes(randomId) ||
          !santa.previousSelectionIds.includes(randomId)
        ) {
          santa.selection = randomId
        }
      }
    },
    distributeAllSantas() {
      this.allParticipantIds.forEach(santa => {
        this.assignSanta(santa)
      })
    },
    showParticipants() {
      // eslint-disable-next-line
      console.log(this.allParticipantIds)
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
      allSantas: []
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
