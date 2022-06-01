<template>
  <div class="">

    <div class="card text-slate-50 bg-sky-800">
      <div class="card-body py-8">
        <div class="row py-10">
          <div class="col-12">
            <!-- Search Bar -->
            <div class="flex justify-left">
              <div class="mb-3 w-100">
                <div class="input-group relative flex flex-wrap items-stretch w-full mb-4">
                  <input type="search"
                    class="py-3 form-control relative flex-auto min-w-0 block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
                    placeholder="Search" aria-label="Search" aria-describedby="button-addon2"
                    v-model="location.name" @keyup.enter="fetchData" />
                  <button
                    class="btn inline-block px-6 py-2.5 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-400 hover:shadow-lg focus:bg-blue-700  focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out flex items-center"
                    type="button" id="button-addon2">
                    <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="search" class="w-4"
                      role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                      <path fill="currentColor"
                        d="M505 442.7L405.3 343c-4.5-4.5-10.6-7-17-7H372c27.6-35.3 44-79.7 44-128C416 93.1 322.9 0 208 0S0 93.1 0 208s93.1 208 208 208c48.3 0 92.7-16.4 128-44v16.3c0 6.4 2.5 12.5 7 17l99.7 99.7c9.4 9.4 24.6 9.4 33.9 0l28.3-28.3c9.4-9.4 9.4-24.6.1-34zM208 336c-70.7 0-128-57.2-128-128 0-70.7 57.2-128 128-128 70.7 0 128 57.2 128 128 0 70.7-57.2 128-128 128z">
                      </path>
                    </svg>
                  </button>
                </div>
              </div>
            </div>
            <!-- Search Bar -->
          </div>
          <div class="col-6 flex justify-start">
            <div class="text-4xl font-semibold text-left">
              <span>{{ currentTemperature.actual }} ºC</span><br>
              <span class="text-2xl font-normal">Feels like {{currentTemperature.feels}} ºC</span>
              <span> <img :src="`http://openweathermap.org/img/w/${this.currentTemperature.icon}.png`"></span>
            </div>
          </div>
          <div class="col-6 flex justify-end text-right">
            <div class="font-normal text-capitalize">
              <span class="text-3xl text-semibold"><i class="fa-solid fa-location-dot pr-1"></i>
                {{location.name}}</span>
              <br>
              <span class="text-2xl"><i class="fa-solid fa-calendar-days pr-2"></i>Thursday</span>
              <br>
              <span class="text-2xl">{{currentTemperature.summary}}</span>
            </div>
          </div>
          <hr class="my-4">
          <div class="col-12">
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
          <hr class="my-4">
          <div class="col-12 mt-2">
            <windStatus :wind_speed="this.wind_speed" :wind_deg="this.wind_deg" />
          </div>

        </div>


      </div>
    </div>

  </div>
</template>

<script>
  import windStatus from './windStatus.vue'
  export default {
    props: ["wind_speed", "wind_deg"],
    components: {
      windStatus
    },
    data() {
      return {
        location: {
          name: 'Guwahati',
          lat: '',
          lng: '',
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
        }
      }
    },
    mounted() {
      // this.fetchData()
    },
    created() {
      this.fetchData()
    },
    methods: {
      fetchData() {
        this.overlay = true
        fetch(
            `https://api.openweathermap.org/data/2.5/weather?appid=35285017a44b049d45eeeca312a04636&q=${this.location.name}&units=metric`
          )
          .then(response => response.json())
          .then(data => {

            console.log(data)
            this.location.lat = data.coord.lat
            this.location.log = data.coord.lon
            this.currentTemperature.actual = Math.round(data.main.temp)
            this.currentTemperature.feels = Math.round(data.main.feels_like)
            this.currentTemperature.summary = data.weather[0].description
            this.currentTemperature.icon = data.weather[0].icon

            this.fetchData1()
            this.show = true
            this.overlay = false
            this.error = false
          })
          .catch(
            (this.error = true),
            (this.show = false),
            (this.show1 = false),
            (this.overlay = false),
          )
      },
      fetchData1() {
        fetch(
            `https://api.openweathermap.org/data/2.5/onecall?lat=${this.location.lat}&lon=${this.location.log}&appid=35285017a44b049d45eeeca312a04636&units=metric`
          )
          .then(response => response.json())
          .then(data => {
            console.log(data)
            // this.daily = data.daily
            // this.show1 = true
            // this.error = false
            this.temp.max = data.daily[0].temp.max
            this.temp.min = data.daily[0].temp.min
          })
      },
      toDayOfWeek(timestamp) {
        const newDate = new Date(timestamp * 1000)
        const days = ['SUN', 'MON', 'TUE', 'WED', 'THURS', 'FRI', 'SAT', 'SUN']

        return days[newDate.getDay()]
      }
    }
  }

</script>


<style scoped>
  .main {
    width: 50%;
    margin: 50px auto;
  }

  .has-search .form-control {
    padding-left: 2.375rem;
  }

  .has-search .form-control-feedback {
    position: absolute;
    z-index: 2;
    display: block;
    width: 2.375rem;
    height: 2.375rem;
    line-height: 2.375rem;
    text-align: center;
    pointer-events: none;
    color: #aaa;
  }

</style>
