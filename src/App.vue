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
      const allParticipantsArray = []
      for (const family in allObj) {
        allObj[family].forEach(personObj => {
          if (personObj.participating) {
            allParticipantsArray.push(personObj.id)
          }
        })
      }
      this.participants = allParticipantsArray
      // eslint-disable-next-line
      console.log(this.participants)
    }
  },
  created: function() {
    this.createParticipants(everyoneObj)
  },
  data() {
    return {
      participants: null
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
