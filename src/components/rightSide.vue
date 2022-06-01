<template>
  <div class="container">
    <div class="row">
      <div class="col-6">
        <div class="card">
          <div class="card-body">
                <p class="underline text-1xl font-bold">UV INDEX</p>
                <uvChart />
          </div>
        </div>
      </div>
      <div class="col-6">
         <div class="card">
          <div class="card-body">
                <p class="underline text-1xl font-bold">AIR VISIBILITY</p>
                <airVisibility :visibility="this.visibility"/>
          </div>
        </div>
      </div>
      <div class="col-12">

      </div>
    </div>
  </div>
</template>

<script>
  import uvChart from './uvChart.vue';
  import airVisibility from './airVisibility.vue'
  export default {
    components: {
      uvChart,
      airVisibility

    },
  }

</script>
