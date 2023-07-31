<template>
  <div class='mainWrapper'>

  <div v-if='!searchOpened' class='overview'>

  <div class='overviewHeader'>
  <button class='overviewSearchButton' @click="searchPlaces()"> Search for places </button>
  <button
            class="material-icons-outlined overviewLocationButton"
            style="font-size: 21px"
          >
            my_location
          </button>
  </div>

<img class='overviewImage' :src="getIcon(weatherToday.icon)">
<p class='overviewTemp'>{{weatherToday.temp_c}}<span class='overviewTempDegrees'>°C</span>
</p>
<p class='overviewDescription'>{{weatherToday.text}}</p>

<p class='currentDate'>Today - {{currentDate}}</p>

<p class='overviewLocation'>
<span class="material-icons" style='font-size:20px'>
location_on 
</span>
<span>London</span>
</p>
  </div>

  <div v-else class='overview'>
 <button @click="searchPlaces()" class='overviewCloseButton'> <span class="material-icons-outlined">
close
</span>
</button>


  <div class='overviewSearch'>
  <span class='searchInputWrapper'>
  <span class="searchIcon material-icons-outlined">
search
</span>
<input class='searchInput'>
</span>
<button class='searchButton'>Search</button>
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
      searchOpened:true,
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
  computed:{
    currentDate(){
      let current = new Date()
      let currentDateString = current.toDateString()
      let day = currentDateString.slice(0,4)
      let month = currentDateString.slice(4,8)
      let date = currentDateString.slice(8,11)

        return day+', '+date + month
    },
    currentLocation(){
      //normally, I would use geolocation and google API to get the city based on the visitor, however, due to security reasons, I don't want my key to be online, so I will hardcode London here
      return "London"
    }
  },
  methods:{
    searchPlaces(){
      this.searchOpened=!this.searchOpened
    },
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
