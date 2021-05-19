<template>
  <div id="app">
    <h1>Total: {{ total }}</h1>

    <label>
      Range:
      <select v-model="range">
        <option value="melee">Melee</option>
        <option value="ranged">Ranged</option>
      </select>
    </label>

    <hr>
    <label>
      <select v-model="adjustements.category">
        <option :value="0">Simple</option>
        <option :value="3">Martial</option>
        <option :value="5">Simple</option>
      </select>
    </label>
    <label>
      <select v-model="adjustements.hands">
        <option :value="0">One Handed</option>
        <option :value="6">Two Handed</option>
      </select>
    </label>
    <label>
      <select v-model="adjustements.die">
        <option :value="3">d4</option>
        <option :value="0">d6</option>
        <option :value="-3">d8</option>
        <option :value="-6">d10</option>
        <option :value="-9">d12</option>
      </select>
    </label>
    <label v-if="!isMelee">
      <select v-model="adjustements.reload">
        <option :value="0">Reload 0</option>
        <option :value="3">Reload 1</option>
        <option :value="6">Reload 2</option>
      </select>
    </label>
    <label v-if="!isMelee">
      <select v-model="adjustements.volley">
        <option :value="0">No Volley</option>
        <option :value="3">Volley 30ft</option>
      </select>
    </label>

    <hr>
    <select v-model="boons.minor" multiple style="height:250px">
      <option value="Backstabber">Backstabber</option>
      <option value="Backswing">Backswing</option>
      <option value="Disarm ">Disarm </option>
      <option value="Finesse">Finesse</option>
      <option value="Versatile">Versatile</option>
      <option value="Free hand">Free hand</option>
      <option value="Propulsive ">Propulsive </option>
      <option value="Shove">Shove</option>
      <option value="Sweep">Sweep</option>
      <option value="Thrown">Thrown</option>
      <option value="Twin">Twin</option>
      <option value="Two handed">Two handed</option>
    </select>
    <select v-model="boons.greater" multiple style="height:250px">
      <option value="Agile">Agile</option>
      <option value="Deadly">Deadly</option>
      <option value="Grapple">Grapple</option>
      <option value="Jousting">Jousting</option>
      <option value="Parry">Parry</option>
      <option value="Range">Range</option>
      <option value="Trip">Trip</option>
      <option value="Forceful">Forceful</option>
    </select>
    <select v-model="boons.major" multiple style="height:250px">
      <option value="Attached">Attached</option>
      <option value="Fatal">Fatal</option>
      <option value="Reach">Reach</option>
    </select>
  </div>
</template>

<script>


export default {
  data: () => ({
    range: 'melee',
    adjustements: {
      category: 0,
      die: 0,
      hands: 0,
      reload: 0,
      volley: 0
    },
    boons: {
      minor: [],
      greater: [],
      major: [],
    },
  }),
  computed: {
    isMelee () {
      return this.range === 'melee';
    },
    total () {
      let value = 1 + this.adjustements.category + this.adjustements.die + this.adjustements.hands
      if(!this.isMelee) {
        value =  value + this.adjustements.reload - 2 + this.adjustements.volley
      }
      value = value - this.boons.minor.length - this.boons.greater.length * 2 - this.boons.major.length * 3
      return value
    }
  },
  name: 'App',
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
