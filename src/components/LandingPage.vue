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

<!-- <img src='https://cdn.weatherapi.com/weather/64x64/day/296.png'> -->

<!-- <span>{{"https"+weatherToday.icon}}</span> -->
<img src={{getIcon(weatherToday.icon)}}>
<span>{{weatherToday.text}}</span>
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
      weatherObj:{},
      weatherToday:{
          icon:'',
          text:'',
          humidity:'',
          wind_mph:'',
          wind_dir:'',
          vis_miles:'',
          pressure_in:'',
          temp_c:'',
          temp_f:'',
      }
    }
  },
  mounted(){
    console.log('brr')
    this.getWeather()
  },
  methods:{
    getIcon(a){
      console.log("https:"+a)
      return "https:"+a
    },
       async getWeather() {

    const response = await fetch('https://api.weatherapi.com/v1/forecast.json?key=51b610f3b328481a908142828233107&q=London&days=3&aqi=no&alerts=no')
    const json =  await response.json();
    this.weatherObj = json

    this.weatherToday={
          icon:json.current.condition.icon,
          text:json.current.condition.text,
          humidity:json.current.humidify,
          wind_mph:json.current.wind_mph,
          wind_dir:json.current.wind_dir,
          vis_miles:json.current.vis_miles,
          pressure_in:json.current.pressure_in,
          temp_c:json.current.temp_c,
          temp_f:json.current.temp_f,
      }

    console.log(this.weatherObj, 'm')
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
