<template>
  <div class='mainWrapper'>

  <div class='overview'>

  <div class='overviewHeader'>
  <button class='overviewSearchButton'> Search for places </button>
  <button
            class="material-icons-outlined overviewLocationButton"
            style="font-size: 21px"
          >
            my_location
          </button>
  </div>

  </div>



  <div class='details'>

  </div>




  </div>
</template>

<script>
export default {
  name: 'LandingPage',
  data(){
    return{
      weatherObj:{}
    }
  },
  mounted(){
    this.getWeather()
  },
  methods:{
        getWeather() {
      fetch('http://api.weatherapi.com/v1/forecast.json?key=51b610f3b328481a908142828233107&q=London&days=3&aqi=no&alerts=no')
        .then((response) => response.json())
        .then((response) => {this.weatherObj = response; console.log(response)});
      
      console.log(this.weatherObj)
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
