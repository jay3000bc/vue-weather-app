<template>
  <div class="m-2" id="app">
    <div class="container-fluid">
      <div class="row">
        <div class="col-12 pb-2">
          <span class="font-bold underline decoration-dashed  text-4xl">Weather Forecast</span>
        </div>
        <div class="col-sm-12 col-md-12 col-lg-4 col-xl-4">
          <!-- <left :wind_speed="this.wind_speed" :wind_deg="this.wind_deg"/> -->
          <!-- Left Side -->
          <div class="card text-slate-50 bg-sky-800">
            <div class="card-body py-9">
              <div class="row py-12">
                <div class="col-12 py-1">
                  <!-- Search Bar -->

                  <div class="flex justify-left">
                    <div class="mb-3 w-100">
                      <div class="input-group relative flex flex-wrap items-stretch w-full mb-4">
                        <input type="search" list="europe-countries"
                          class="py-3 form-control relative flex-auto min-w-0 block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
                          placeholder="Search" aria-label="Search" aria-describedby="button-addon2"
                          v-model="location.name" @keyup.enter="fetchData" @change="fetchData"/>
                        <button @click="fetchData"
                          class="btn inline-block px-6 py-2.5 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-400 hover:shadow-lg focus:bg-blue-700  focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out flex items-center"
                          type="button" id="button-addon2">
                          <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="search" class="w-4"
                            role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                            <path fill="currentColor"
                              d="M505 442.7L405.3 343c-4.5-4.5-10.6-7-17-7H372c27.6-35.3 44-79.7 44-128C416 93.1 322.9 0 208 0S0 93.1 0 208s93.1 208 208 208c48.3 0 92.7-16.4 128-44v16.3c0 6.4 2.5 12.5 7 17l99.7 99.7c9.4 9.4 24.6 9.4 33.9 0l28.3-28.3c9.4-9.4 9.4-24.6.1-34zM208 336c-70.7 0-128-57.2-128-128 0-70.7 57.2-128 128-128 70.7 0 128 57.2 128 128 0 70.7-57.2 128-128 128z">
                            </path>
                          </svg>
                        </button>
                        <datalist id="europe-countries">
                          <!-- <option v-for="state in cityList" :key="state.id">
                                {{state.name}}
                              </option> -->
                          <!-- <div v-for="country in jsonList" :key="country.id">
                            <option>
                              {{country.name}}
                            </option>
                            <div v-for="state in country.states" :key="state.id">
                              <option>{{state.name}}</option>
                              <option v-for="city in state.cities" :key="city.id">{{city.name}}</option>
                            </div>

                          </div> -->
                          <!-- <option v-for="country in country" :key="country.id">{{country}}</option>
                          <option v-for="state in state" :key="state.id">{{state}}</option> -->
                          <!-- <option v-for="state in state" :key="state.id">{{state}}</option> -->
                          <option v-for="country in country" :key="country.id">{{country}}</option>
                          <option v-for="city in city" :key="city.id">{{city}}</option>
                        </datalist>
                      </div>


                    </div>
                  </div>
                  <!-- Search Bar -->
                </div>
                <div class="col-6 flex justify-start" v-if="!this.error">
                  <div class="text-4xl font-semibold text-left">
                    <span>{{ currentTemperature.actual }} ºC</span><br>
                    <span class="text-2xl font-normal">Feels like {{currentTemperature.feels}} ºC</span>
                    <span> <img :src="`http://openweathermap.org/img/w/${this.currentTemperature.icon}.png`"></span>
                  </div>
                </div>
                <div class="col-6 flex justify-end text-right" v-if="!this.error">
                  <div class="font-normal text-capitalize">
                    <span class="text-3xl text-semibold"><i class="fa-solid fa-location-dot pr-1"></i>
                      {{location.name}}</span>
                    <br>
                    <span class="text-2xl"><i class="fa-solid fa-calendar-days pr-2"></i>Thursday</span>
                    <br>
                    <span class="text-2xl">{{currentTemperature.summary}}</span>
                  </div>
                </div>
                <hr class="my-4" v-if="!this.error">
                <div class="col-12" v-if="!this.error">
                  <div class="text-3xl">
                    <span class="pr-4">
                      <i class="fa-solid fa-caret-up"></i>
                      {{ Math.round(this.temp.max) }} ºC
                    </span>
                    <span class="pl-4">
                      <i class="fa-solid fa-caret-down"></i>
                      {{ Math.round(this.temp.min) }} ºC
                    </span>
                  </div>
                </div>
                <hr class="my-4" v-if="!this.error">
                <div class="col-12 mt-2" v-if="!this.error">
                  <windStatus :wind_speed="this.wind_speed" :wind_deg="this.wind_deg" />
                </div>

                <div class="col-12" v-if="this.error">
                  Sorry No Data Found !
                </div>

              </div>


            </div>
          </div>
          <!-- Left Side -->
        </div>
        <div class="col-sm-12 col-md-12 col-lg-8 col-xl-8" >
          <div class="container-fluid m-0 p-0">
            <div class="row">
              <div class="col-sm-12 col-md-12 col-lg-6 col-xl-6 mt-2">
                <div class="card flex justify-center">
                  <div class="card-body">
                    <p class="underline text-1xl font-bold">UV INDEX</p>
                    <uvChart v-if="this.uvi" :uvi="this.uvi" />
                    <!-- <uvChart v-else-if="this.loading && this.uvi==0" :uvi="this.uvi" /> -->
                  </div>
                </div>
              </div>
              <div class="col-sm-12 col-md-12 col-lg-6 col-xl-6 mt-2">
                <div class="card">
                  <div class="card-body">
                    <p class="underline text-1xl font-bold">AIR VISIBILITY</p>
                    <airVisibility v-if="this.visibility" :visibility="this.visibility" />
                  </div>
                </div>
              </div>
              <div class="col-12 mt-2">
                <div class="card">
                  <div class="card-body">
                    <p class="underline text-1xl font-bold">HOURLY TEMPERATURE</p>
                    <temperatureChart v-if="this.chartData && !this.error" :chartData="this.chartData" />
                  </div>
                </div>
              </div>
            </div>
          </div>

        </div>
      </div>

    </div>
    <div v-if="loading" class="loading">Loading&#8230;</div>
  </div>
</template>

<script>
  import temperatureChart from './components/temperatureChart.vue'
  import uvChart from './components/uvChart.vue';
  import airVisibility from './components/airVisibility.vue'
  import windStatus from './components/windStatus.vue';

  // import left from './components/leftSide.vue';
  import right from './components/rightSide.vue';

  // import json from '../city.json';
  import auto from './components/auto.vue'


  export default {
    name: 'app',
    components: {
      temperatureChart,
      uvChart,
      airVisibility,
      windStatus,
      // left,
      right,
      auto
    },
    data() {
      return {
        loading: false,
        msg: 'Welcome to Your Vue.js App',
        weather_data: "",
        wind_speed: "",
        wind_deg: "",
        visibility: '',
        chartData: [],
        location: {
          name: 'Guwahati',
          lat: '',
          lon: '',
        },
        currentTemperature: {
          actual: '',
          feels: '',
          summary: '',
          icon: '',
        },
        daily: [],
        show: false,
        show1: false,
        overlay: false,
        error: false,
        temp: {
          max: '',
          min: '',
        },
        uvi: '',
        // jsonList: json,
        country: [],
        state: [],
        city: [],
      }
    },
    created() {
      
      this.fetchData()
      this.getCountry()
      // this.getState()


      //  fetch(
      //       `https://countriesnow.space/api/v0.1/countries`
      //     )
      //     .then(response => response.json())
      //     .then(data => {
      //       console.log("check",data)
      //     })

    },
    mouted() {
      this.fetchData()
    },
    methods: {
      secondsToHms(t) {
        var dt = new Date(t * 1000);
        var hr = dt.getHours();
        var m = "0" + dt.getMinutes();
        var s = "0" + dt.getSeconds();
        return hr + ':' + m.substr(-2) + ':' + s.substr(-2);
      },
      fetchData() {
        this.overlay = true
        this.visibility = '';
        this.uvi = '';
        this.chartData = [];
        this.loading = true
        fetch(
            `https://api.openweathermap.org/data/2.5/weather?appid=35285017a44b049d45eeeca312a04636&q=${this.location.name}&units=metric`
          )
          .then(response => response.json())
          .then(data => {

            if (data.cod == 400) {
              this.visibility = '';
              this.uvi = '';
              this.chartData = [];
              this.error = true;
            }

            if (data.cod == 200) {
              // console.log("Current Data", data)
              this.location.lat = data.coord.lat
              this.location.lon = data.coord.lon
              this.fetchData1()

              this.currentTemperature.actual = Math.round(data.main.temp)
              this.currentTemperature.feels = Math.round(data.main.feels_like)
              this.currentTemperature.summary = data.weather[0].description
              this.currentTemperature.icon = data.weather[0].icon
              this.wind_speed = data.wind.speed.toString()
              this.wind_deg = data.wind.deg
              this.error = false;
            } else {
              this.visibility = '';
              this.uvi = '';
              this.chartData = [];
              this.location.lat = '0';
              this.location.lon = '0';
            }

            this.loading = false


          })
          .catch(
            (this.error = true),
          )
      },
      fetchData1() {
        fetch(
            `https://api.openweathermap.org/data/2.5/onecall?lat=${this.location.lat}&lon=${this.location.lon}&appid=35285017a44b049d45eeeca312a04636&units=metric`
          )
          .then(response => response.json())
          .then(data => {

            this.visibility = data.current.visibility / 1000
            // console.log("Hour Data", data)
            this.temp.max = data.daily[0].temp.max
            this.temp.min = data.daily[0].temp.min

            // this.uvi = data.current.uvi
             this.uvi =`${data.current.uvi}`
            // this.uvi = `0`
            // console.log("uvi",data.current.visibility/1000)


            // var hour = [];
            for (var i = 0; i < data.hourly.length; i++) {
              var dataObject = {
                // label: data.hourly[i].dt,
                label: this.secondsToHms(data.hourly[i].dt),
                value: data.hourly[i].temp,
              }
              this.chartData.push(dataObject)
            }


          })
      },
      toDayOfWeek(timestamp) {
        const newDate = new Date(timestamp * 1000)
        const days = ['SUN', 'MON', 'TUE', 'WED', 'THURS', 'FRI', 'SAT', 'SUN']

        return days[newDate.getDay()]
      },

      getCountry() {
        // for (var i = 0; i < this.jsonList.length; i++) {
        //   this.country.push(this.jsonList[i].name)
        // }
        // // console.log("Country",this.country)

        
       fetch(
            `https://countriesnow.space/api/v0.1/countries`
          )
          .then(response => response.json())
          .then(data => {

            // console.log("check",data)
            for(var c in data.data){
              this.country.push(data.data[c].country)

              for(var ct in data.data[c].cities){
                this.city.push(data.data[c].cities[ct])
              }
            }

            // console.log("city",this.city)
     
          })


      },
      getState() {
        console.log("test")
        // for (var c in this.jsonList){
        //     for(var st in this.jsonList[c].states){
        //       // console.log("st",this.jsonList[c].states[st].name)
        //       this.state.push(this.jsonList[c].states[st].name)

        //       for(var ct in this.jsonList[c].states[st].cities){
        //         this.city.push(this.jsonList[c].states[st].cities[ct].name)
        //       }
        //     }
        // }
        // //console.log("city",this.city)
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

  h1,
  h2 {
    font-weight: normal;
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


  /* Absolute Center Spinner */
  .loading {
    position: fixed;
    z-index: 999;
    height: 2em;
    width: 2em;
    overflow: show;
    margin: auto;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
  }

  /* Transparent Overlay */
  .loading:before {
    content: '';
    display: block;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: radial-gradient(rgba(20, 20, 20, .8), rgba(0, 0, 0, .8));

    background: -webkit-radial-gradient(rgba(20, 20, 20, .8), rgba(0, 0, 0, .8));
  }

  /* :not(:required) hides these rules from IE9 and below */
  .loading:not(:required) {
    /* hide "loading..." text */
    font: 0/0 a;
    color: transparent;
    text-shadow: none;
    background-color: transparent;
    border: 0;
  }

  .loading:not(:required):after {
    content: '';
    display: block;
    font-size: 10px;
    width: 1em;
    height: 1em;
    margin-top: -0.5em;
    -webkit-animation: spinner 150ms infinite linear;
    -moz-animation: spinner 150ms infinite linear;
    -ms-animation: spinner 150ms infinite linear;
    -o-animation: spinner 150ms infinite linear;
    animation: spinner 150ms infinite linear;
    border-radius: 0.5em;
    -webkit-box-shadow: rgba(255, 255, 255, 0.75) 1.5em 0 0 0, rgba(255, 255, 255, 0.75) 1.1em 1.1em 0 0, rgba(255, 255, 255, 0.75) 0 1.5em 0 0, rgba(255, 255, 255, 0.75) -1.1em 1.1em 0 0, rgba(255, 255, 255, 0.75) -1.5em 0 0 0, rgba(255, 255, 255, 0.75) -1.1em -1.1em 0 0, rgba(255, 255, 255, 0.75) 0 -1.5em 0 0, rgba(255, 255, 255, 0.75) 1.1em -1.1em 0 0;
    box-shadow: rgba(255, 255, 255, 0.75) 1.5em 0 0 0, rgba(255, 255, 255, 0.75) 1.1em 1.1em 0 0, rgba(255, 255, 255, 0.75) 0 1.5em 0 0, rgba(255, 255, 255, 0.75) -1.1em 1.1em 0 0, rgba(255, 255, 255, 0.75) -1.5em 0 0 0, rgba(255, 255, 255, 0.75) -1.1em -1.1em 0 0, rgba(255, 255, 255, 0.75) 0 -1.5em 0 0, rgba(255, 255, 255, 0.75) 1.1em -1.1em 0 0;
  }

  /* Animation */

  @-webkit-keyframes spinner {
    0% {
      -webkit-transform: rotate(0deg);
      -moz-transform: rotate(0deg);
      -ms-transform: rotate(0deg);
      -o-transform: rotate(0deg);
      transform: rotate(0deg);
    }

    100% {
      -webkit-transform: rotate(360deg);
      -moz-transform: rotate(360deg);
      -ms-transform: rotate(360deg);
      -o-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }

  @-moz-keyframes spinner {
    0% {
      -webkit-transform: rotate(0deg);
      -moz-transform: rotate(0deg);
      -ms-transform: rotate(0deg);
      -o-transform: rotate(0deg);
      transform: rotate(0deg);
    }

    100% {
      -webkit-transform: rotate(360deg);
      -moz-transform: rotate(360deg);
      -ms-transform: rotate(360deg);
      -o-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }

  @-o-keyframes spinner {
    0% {
      -webkit-transform: rotate(0deg);
      -moz-transform: rotate(0deg);
      -ms-transform: rotate(0deg);
      -o-transform: rotate(0deg);
      transform: rotate(0deg);
    }

    100% {
      -webkit-transform: rotate(360deg);
      -moz-transform: rotate(360deg);
      -ms-transform: rotate(360deg);
      -o-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }

  @keyframes spinner {
    0% {
      -webkit-transform: rotate(0deg);
      -moz-transform: rotate(0deg);
      -ms-transform: rotate(0deg);
      -o-transform: rotate(0deg);
      transform: rotate(0deg);
    }

    100% {
      -webkit-transform: rotate(360deg);
      -moz-transform: rotate(360deg);
      -ms-transform: rotate(360deg);
      -o-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }

</style>
