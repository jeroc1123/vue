<template>
  <div id="app">
    <h1>Ejemplo aumentar reducir y asignar</h1>
    <h2>Contador {{ counter }}</h2>
    <button @click="increment">incrementar</button>
    <button @click="decrement">restar</button>
    <button @click='asignar'>asignar</button>
    <input v-model="cantidad" />
    <h1>Ejemplo Ajax. Precio de Bitcoin</h1>

    <section v-if="errored">
      <p>Lo sentimos, no es posible obtener la información en este momento, por favor intente nuevamente mas tarde</p>
    </section>

    <section v-else>
      <div v-if="loading">Cargando...</div>

      <div v-else v-for="currency in info" :key="currency.id" class="currency">
        {{ currency.description }}:
        <span class="lighten">
          <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
        </span>
      </div>

    </section>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "App",
  data() {
    return {
      counter: 0,
      cantidad: 0,
      info: '',
      loading: true,
      errored: false
    };
  },
  methods: {
    increment() {
      // update component state
      this.counter++;
    },
    decrement() {
      this.counter--;
    },
    asignar(){
      this.counter = this.cantidad
    }, 
  },
  mounted () {
      axios
        .get('https://api.coindesk.com/v1/bpi/currentprice.json')
        .then(response => {
          this.info = response.data.bpi
        })
        .catch(error => {
          console.log(error)
          this.errored = true
        })
        .finally(() => this.loading = false)
    },
  filters: {
    currencydecimal (value) {
      return value.toFixed(2)
    }
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  display: flex;
  justify-content: center;
  background: #7E8D85;
  font-family: "Roboto Slab", serif;
  line-height: 1.4;
}

#app {
  margin-top: 20px;
  width: 300px;
  padding: 0 40px 40px;
  background: #2F242C;
  border-radius: 5px;
  color: #B3BFB8;
}

h1 {
  color: #F0F7F4;
}

.lighten {
  color: white;
}

p {
  margin: 0;
}
</style>
