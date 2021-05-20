<template>
  <div id="app" class="container">
    <form>
      <h1>Points Left: {{ total }}</h1>

      <div class="form-check">
        <input v-model="range" class="form-check-input" type="radio" id="range-melee" value="melee">
        <label class="form-check-label" for="range-melee">
          Melee
        </label>
      </div>
      <div class="form-check">
        <input v-model="range" class="form-check-input" type="radio" id="range-ranged" value="ranged">
        <label class="form-check-label" for="range-ranged">
          Ranged
        </label>
      </div>

      <hr>
        <label class="me-2">
          Expensive (13gp+)
          <select class="form-select block" v-model="adjustements.price">
            <option :value="0">Nope</option>
            <option :value="1">Yup</option>
          </select>
        </label>
        <label class="me-2">
          Weapon Category
          <select class="form-select block" v-model="adjustements.category">
            <option :value="0">Simple</option>
            <option :value="3">Martial</option>
            <option :value="5">Advanced</option>
          </select>
        </label>
        <label class="me-2">
          Hands
          <select class="form-select block" v-model="adjustements.hands">
            <option :value="0">1</option>
            <option v-if="!isMelee" :value="1">1+</option>
            <option :value="6">2</option>
          </select>
        </label>
        <label class="me-2">
          Damage
          <select class="form-select" v-model="adjustements.die">
            <option :value="3">d4</option>
            <option :value="0">d6</option>
            <option :value="-3">d8</option>
            <option :value="-6">d10</option>
            <option :value="-9">d12</option>
          </select>
        </label>
        <label v-if="!isMelee" class="me-2">
          Reload
          <select class="form-select" v-model="adjustements.reload">
            <option :value="0">Reload 0</option>
            <option :value="3">Reload 1</option>
            <option :value="6">Reload 2</option>
          </select>
        </label>
      <label v-if="!isMelee" class="me-2">
        Range
        <select class="form-select" v-model="adjustements.range">
          <option :value="4">20ft</option>
          <option :value="3">30ft</option>
          <option :value="1">50ft</option>
          <option :value="0">60ft</option>
          <option :value="-2">100ft</option>
          <option :value="-3">120ft</option>
        </select>
      </label>
        <label v-if="!isMelee" class="me-2">
          Volley
          <select class="form-select" v-model="adjustements.volley">
            <option :value="0">No Volley</option>
            <option :value="3">Volley 30ft</option>
          </select>
        </label>
      <hr>
      <div class="row">
        <div class="col">
          Minor Boons (1 point each)
          <select class="form-select" v-model="boons.minor" multiple>
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
        </div>
        <div class="col">
          Greater Boons (2 points each)
          <select class="form-select" v-model="boons.greater" multiple>
            <option value="Agile">Agile</option>
            <option value="Deadly">Deadly</option>
            <option value="Grapple">Grapple</option>
            <option value="Jousting">Jousting</option>
            <option value="Parry">Parry</option>
            <option value="Trip">Trip</option>
          </select>
        </div>
        <div class="col">
          Major Boons (3 points each)
          <select class="form-select" v-model="boons.major" multiple>
            <option value="Attached">Attached</option>
            <option value="Fatal">Fatal</option>
            <option value="Reach">Reach</option>
            <option value="Forceful">Forceful</option>
            <option value="Repeating">Repeating</option>
          </select>
        </div>
      </div>
    </form>
    <hr />
    <div v-if="hasFinesse">
      <h4>Finesse</h4>
      <p>There are currently no published weapons with Finesse trait.</p>
    </div>
    <div>
      <h4>Source</h4>
      <p>The implemented calculation has not been in any way confirmed by Paizo to be anything close to real.</p>
      <p>The tool is built on top of the analysis made by Pronate (see <a href="https://docs.google.com/document/d/1j0uUtVcTgvn2a0oMYFKMwe_-tAPOdnFY21_0FOiX2DI/edit">the document</a> for details) with several modifications on top.</p>
    </div>
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
      volley: 0,
      price: 0,
      range: 0,
    },
    boons: {
      minor: [],
      greater: [],
      major: [],
    },
  }),
  computed: {
    hasFinesse () {
      return this.boons.minor.indexOf('Finesse') > -1
    },
    isMelee () {
      return this.range === 'melee';
    },
    total () {
      let value = 1 + this.adjustements.category + this.adjustements.die + this.adjustements.hands + this.adjustements.price
      if(!this.isMelee) {
        value =  value - 3 + this.adjustements.reload + this.adjustements.volley + this.adjustements.range
      }
      value = value - this.boons.minor.length - this.boons.greater.length * 2 - this.boons.major.length * 3
      return value
    }
  },
  name: 'App',
}
</script>