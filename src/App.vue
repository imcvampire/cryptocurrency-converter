<template>
  <div id="app">
    <div class="container">
      <form>
        <div class="form-group">
          <label for="currency1">First currency</label>
          <input
            type="text"
            class="form-control"
            id="currency1"
            placeholder="Enter currency"
            :value="currency1"
            @input="currency1 = $event.target.value.toUpperCase()"
          >
        </div>

        <div class="form-group">
          <label for="currency2">Second currency</label>
          <input
            type="text"
            class="form-control"
            id="currency2"
            placeholder="Enter currency"
            :value="currency2"
            @input="currency2 = $event.target.value.toUpperCase()"
          >
        </div>

        <div v-show="haveData" class="alert alert-primary" role="alert">
          Exchange Rate between {{ currency1 }} and {{ currency2 }} is {{ exchangeRate }}
        </div>

        <button
          type="submit"
          class="btn btn-primary"
          @click.stop.prevent="getData"
          :disabled="loading"
        >
          {{ loading ? 'Loading...' : 'Submit' }}
        </button>
      </form>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'

  const apiEndpoint = 'https://min-api.cryptocompare.com/data/price'

  export default {
    name: 'app',

    data() {
      return {
        currency1: '',
        currency2: '',
        exchange: 0,
        haveData: false,
        exchangeRate: '',
        loading: false,
      }
    },

    watch: {
      currency1() {
        this.$set(this, 'haveData', false)
      },

      currency2() {
        this.$set(this, 'haveData', false)
      },
    },

    methods: {
      getData() {
        this.$set(this, 'haveData', false)
        this.$set(this, 'loading', true)

        axios.get(apiEndpoint, {
          params: {
            fsym: this.currency1,
            tsyms: this.currency2,
          },
        }).then(({ data }) => {
          this.$set(this, 'exchangeRate', data[this.currency2])

          this.$set(this, 'haveData', true)
          this.$set(this, 'loading', false)
        })
      },
    },
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
