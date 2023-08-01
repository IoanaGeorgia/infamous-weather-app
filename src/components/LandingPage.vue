<template>
  <div class="mainWrapper">
    <div v-if="!searchOpened" class="overview">
      <div class="overviewHeader">
        <button class="overviewSearchButton" @click="searchPlaces()">
          Search for places
        </button>
        <button
          @click="searchMyLocation()"
          class="material-icons-outlined overviewLocationButton"
          style="font-size: 21px"
        >
          my_location
        </button>
      </div>

      <img class="overviewImage" :src="getIcon(weatherToday.icon)" />
      <p class="overviewTemp">
      <span v-if="degrees==='C'"> {{ weatherToday.temp_c }}</span>
      <span v-else> {{ weatherToday.temp_f }}</span>

      <span class="overviewTempDegrees">°{{degrees}}</span>
      </p>
      <p class="overviewDescription">{{ weatherToday.text }}</p>

      <p class="currentDate">Today - {{ currentDate }}</p>

      <p class="overviewLocation">
        <span class="material-icons" style="font-size: 20px">
          location_on
        </span>
        <span>{{ currentLocation }}</span>
      </p>
    </div>

    <div v-else class="overview">
      <button @click="searchPlaces()" class="overviewCloseButton">
        <span class="material-icons-outlined"> close </span>
      </button>

      <div class="overviewSearch">
        <span class="searchInputWrapper">
          <span class="searchIcon material-icons-outlined"> search </span>
          <input v-model.trim="searchLocationInput" class="searchInput" />
        </span>
        <button class="searchButton" @click="searchByLocation()">Search</button>
      </div>
    </div>

    <div class="details">

    <p class='detailsDegrees'>
    <button :class="{ 'degreesButtonSelected': degreesStyling, 'degreesButtonUnselected': !degreesStyling  }" @click='setDegrees("C")'>°C</button>
    <button :class="{ 'degreesButtonSelected': degreesStyling, 'degreesButtonUnselected': !degreesStyling  }" @click='setDegrees("F")'>°F</button>
    </p>
    
    
    </div>
  </div>
</template>

<script>
export default {
  name: 'LandingPage',
  data() {
    return {
      searchOpened: true,
      weatherObj: {},
      searchLocationInput: null,
      //London takes the place of currenttLocation too
      defaultLocation: 'London',
      degrees: "C",
      weatherToday: {
        icon: '',
        text: '',
        humidity: '',
        wind_mph: '',
        wind_dir: '',
        vis_miles: '',
        pressure_in: '',
        temp_c: '',
        temp_f: '',
      },
    };
  },
  mounted() {
    //normally, I would use geolocation and google API to get the city based on the visitor, however, due to security reasons, I don't want my key to be online, so I will hardcode London here
    this.currentLocation = this.defaultLocation;
    this.getWeather(this.currentLocation);
  },
  computed: {
    currentDate() {
      let current = new Date();
      let currentDateString = current.toDateString();
      let day = currentDateString.slice(0, 4);
      let month = currentDateString.slice(4, 8);
      let date = currentDateString.slice(8, 11);

      return day + ', ' + date + month;
    },
    degreesStyling(){
      console.log(this.degrees)
      this.degrees === "C" ? true : false
    }
  },
  methods: {
    setDegrees(deg){
        this.degrees=deg
    },
    searchMyLocation() {
      // due to security reasons, I won't be using the geolocator here ( I don't want my google api key online), however, this would use the built-in Js geolocator to find the coordonates and a separate api to translate them to a city (the browser would ask the viewer if they're ok with sharing their location before this action takes place)
      this.getWeather(this.defaultLocation);
      this.currentLocation = this.defaultLocation;
    },
    searchByLocation() {
      //  normally, autocomplete and error handling for the location input would be implemented using ( I would choose Google API places autocomplete), but due to security reasons I won't be putting my google api key here. no autocomplete and error handling here
      if (this.searchLocationInput) {
        this.getWeather(this.searchLocationInput);
        this.currentLocation = this.searchLocationInput;
      }
    },
    searchPlaces() {
      this.searchOpened = !this.searchOpened;
    },
    getIcon(path) {
      return 'https:' + path;
    },
    async getWeather(loc) {
      const response = await fetch(
        `https://api.weatherapi.com/v1/forecast.json?key=51b610f3b328481a908142828233107&q=${loc}&days=3&aqi=no&alerts=no`);
      const json = await response.json();
      this.weatherObj = json;
      this.weatherToday = {
        icon: json.current.condition.icon,
        text: json.current.condition.text,
        humidity: json.current.humidify,
        wind_mph: json.current.wind_mph,
        wind_dir: json.current.wind_dir,
        vis_miles: json.current.vis_miles,
        pressure_in: json.current.pressure_in,
        temp_c: json.current.temp_c,
        temp_f: json.current.temp_f,
      };
    },
  },
};
</script>

