<template>
  <!-- <div class="highlights-item col-md-4 col-sm-6 col-xs-12 border-top">
   <div>
   <div class="card-heading pt-5">Wind Status</div>
   <div class="row pt-4 mt-4">
     <div class="col-sm-6 col-md-6 mt-2 text-center align-middle">
       <p class="card-sub-heading mt-3">Wind Direction</p>
       <p class="mt-4"><img src="../assets/winddirection.svg" height="40" width="40"></p>
       <p class="card-value mt-4">{{this.direction}}</p>
     </div>
     <div class="col-sm-6 col-md-6 mt-2">
       <p class="card-sub-heading mt-3">Wind Speed</p>
       <p class="mt-4"><img src="../assets/windspeed.svg" height="40" width="40"></p>
       <p class="card-value mt-4">{{this.wind_speed}} km/h</p>
     </div>
   </div>
   </div>
 </div> -->
  <div>
    <div class="row text-slate-50">
      <div class="col-6 ">

        <span class="text-lg">Wind Direction</span><br>
        <span>
          <i class="fa-solid fa-wind text-3xl"></i>
        </span><br>
        <span class="text-lg">{{this.direction}}</span>

      </div>
      <div class="col-6">
        <span class="text-lg">Wind Speed</span><br>
        <span>
          <i class="fa-solid fa-gauge-max"></i>
        </span><br>
        <span class="text-lg">{{this.wind_speed}} km/h</span>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ["wind_speed", "wind_deg"],
    data() {
      return {
        direction: '',
        //  degrees:'',

      }
    },
    created() {
      this.findDirection()
    },
    methods: {
      findDirection() {
        // Insert the amount of degrees here
        var degrees = this.wind_deg;

        // Define array of directions
        var directions = ['North', 'Northeast', 'East', 'Southeast', 'South', 'Southwest', 'West', 'Northwest'];

        // Split into the 8 directions
        degrees = degrees * 8 / 360;

        // round to nearest integer.
        degrees = Math.round(degrees, 0);

        // Ensure it's within 0-7
        degrees = (degrees + 8) % 8

        // console.log(directions[degrees])

        this.direction = directions[degrees]
      }
    },
  }

</script>
