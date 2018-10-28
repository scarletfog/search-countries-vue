<template>
  <div id="app">
    <img src="./assets/logo.png" width="75px" height="75px">
    <h1 class="mt-3">Vue.js Search countries</h1>
    <div class="container">

      <section>
         <div class="row justify-content-center mt-4">
           <input v-model="inputField" v-on:keyup.enter="searchCountry" class="mr-1" placeholder="Look the country up" />
           <button @click="searchCountry" class="btn btn-primary">Add the country!</button>
        </div>
      </section>

       <section class="container">
        <div v-if="loading">
                     <div class="wrapper">
      <section class="dots">
        <div class="dot one"></div>
        <div class="dot two"></div>
        <div class="dot three"></div>
      </section>
      <section class="box-shadow">
        <div class="shadow one"></div>
        <div class="shadow two"></div>
        <div class="shadow three"></div>
      </section>
        </div>
        <div v-if="!loading">
        </div>
  </div>

          <div class="row">
             <div class="offset-md-3 col-md-6 mt-3">
                       <div v-if="error">
                         Error fetching data, please try again
        </div>
                <ul class="list-group justify-content-center">
                   <li class="row country list-group-item border mt-2 col-xs-1" v-for="country in countriesList">
                      <div class="row align-items-center">
                        <span  class="col-sm-8">
                           <h5><span class="flag"><img v-bind:src="`${country.flag}`" alt="Flag" width="100"></span>Country name: {{ country.name }}</h5>
                           <div class="dataWrapper">
                           <div>The capital: {{ country.capital }}</div>
                           <div>Currency: {{ country.currency }}</div>
                           <div>Language: {{ country.language }}</div>
                           </div>
                        </span>
                        <span @click="deleteCountry(country)" class="offset-sm-1 col-sm-2 delete text-right">X</span>
                      </div>
                   </li>
                </ul>
             </div>
          </div>
       </section>

    </div>

  </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

const api = 'https://restcountries.eu/rest/v2/name/'

export default {
  name: 'app',
  components: {
  },
  data () {
    return {
      inputField: '',
      countriesList: [],
      loading: true,
      error: false
    }
  },
  methods: {

    deleteCountry: function (country) {
      const index = this.countriesList.indexOf(country)
      this.countriesList.splice(index, 1)
      this.countriesList.length ? '' : this.loading = true
    },
    searchCountry: function (countryName) {
      countryName = this.inputField
      if (!countryName.length) countryName = 'Poland'

      axios
        .get(api + countryName)

        .then((res) => {
          const data = res.data.shift();

          ((
            this.loading = false,
            this.error = false,
            this.countriesList.push(

              {
                name: data.name,
                capital: data.capital,
                currency: data.currencies.map(i => i.code).toString(),
                language: data.languages.map(i => i.iso639_1).toString(),
                flag: data.flag
              }

            )
          )

          )
        }

        )
        .catch((err) => {
          ((this.error = true))
        })
    }
  }

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
body {
  background-color: #fff;

}
h1, h2 {
  font-weight: normal;
}

h5 {
   margin-bottom: 0px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.delete {
   color: pink;
   cursor: pointer;
}

.delete:hover {
   color: red;
}

.country {
  background-color: #f5f5f5;
}

.flag {
  float: left;
}

.dataWrapper {
  padding-top: 20px;
    float: right;
}
/* for loader animation*/
.wrapper {
  width: 80%;
  text-align: center;
  margin: auto;
  margin-top: 10vh;
}

.box-shadow {
  display: block;
  margin: auto;
  width: 80%;
  line-height: 106px;
  animation-name: opa;
  animation-duration: 0.6s;
  animation-iteration-count: 1;
}

.dot {
  display: inline-block;
  width: 30px;
  height: 30px;
  background-color: #1abc9c;
  border: none;
  border-radius: 50%;
  margin: 0 2px;
  animation-name: wave;
  animation-duration: 0.5s;
  animation-timing-function: cubic-bezier(0.64, 0.12, 0.79, 0.35);
  animation-iteration-count: infinite;
  animation-direction: alternate;
}

.dot:nth-child(2) {
  background-color: #304960;
}

.shadow {
  display: inline-block;
  text-align: center;
  margin: 0 2px;
  border-radius: 50%;
  opacity: 0.5;
  width: 30px;
  height: 5px;
  background-color: rgba(11, 15, 19, 0.42);
  animation-name: strech ;
  animation-duration: 0.5s;
  animation-timing-function: cubic-bezier(0.64, 0.12, 0.79, 0.35);
  animation-iteration-count: infinite;
  animation-direction: alternate;
}

@keyframes wave {
  /* 0% {transform: translateY(0);} */
  0% {transform: translateY(0px);}
  100% {transform: translateY(60px);}
}

@keyframes strech {
  0% {transform: scale(0)}
  100% {transform: scale(1)}
}

@keyframes opa {
  0% {opacity: 0;}
  99% {opacity: 0;}
  100% {opacity: 1;}
}

.dot.two, .shadow.two {
  animation-delay: 0.3s;
}

.dot.three, .shadow.three {
  animation-delay: 0.6s;
}
</style>
