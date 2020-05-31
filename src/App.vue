<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp >16 ? 'warm' : ''">
   <main>
     <div class="search-box">
       <input type="text"  class="search-bar" 
       placeholder="Search..."
       v-model="query"
       @keypress="fetchWeather"
       />
     </div>
     <div class="notFound" v-if="typeof weather.main == 'undefined'">
        {{weather.message}}
     </div>
     <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
       <div class="location-box">
         <div class="location">{{weather.name}} , {{weather.sys.country}} , {{weather.message}} </div>
         <div class="date">{{ dateBuilder() }}  </div>
       </div>
       <div class="weather-box">
         <div class="temperature"> {{weather.main.temp}} C</div>
         <div class="status"> {{weather.weather[0].description}} </div>
       </div>
     </div>
   </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data () {
    return {
      api_key : '717fea70a50e5750f123082793e06d96',
      url_base : 'https://api.openweathermap.org/data/2.5/',
      query : '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(x) {
        if(x.key == "Enter"){
          fetch(`${this.url_base}weather?q=${this.query}&&units=metric&&APPID=${this.api_key}`)
          .then( res => {
            return res.json();
          }).then(this.setResults);
        }
    },
    setResults(d) {
        this.weather = d;
        
    },
    dateBuilder() {
      let d = new Date();
      
      let month = d.getMonth();
      let year = d.getFullYear();
      let date = d.getDate();

      return ` ${date} - ${month} - ${year}  `
    },
    
  }
}
</script>

<style>
* {
   margin: 0px;
   padding: 0px;
   box-sizing: border-box;
}
#app {
  background-image: url('./assets/cold-image.jpg');
  background-size: cover;
  background-position: bottom;
}

#app.warm {
  background-image: url('./assets/hot-temperature.jpg');
}
main{
  min-height: 100vh;
  padding: 30px;
}
.search-box{
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  display: block;
  padding: 15px;
  font-size: 20px ;
}
.location-box .location{
  color:royalblue;
  font-size: 30px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date{
  color:black;
  font-size: 22px;
  font-weight: 400;
  text-align: center;
  font-style: italic;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.weather-box{
  text-align: center;
  
}
.weather-box .temperature{
  display: inline-block;
  padding: 10px 25px;
  font-size: 100px;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  color:slateblue;
  background-color: rgba(255, 255, 255, 0.25);
  font-weight: 900;
  margin: 30px 0px;
  border-radius: 0px 3px 6px 9px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .status{
  
  padding: 10px 25px;
  font-size: 50px;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  color:slateblue;
  
  font-weight: 500;
  margin: 30px 0px;
  
}
.notFound {
  text-align:center;
  font-family: 'Times New Roman', Times, serif;
  font-size: 100px;
  font-weight: 900px;
  color: hotpink;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);

}
</style>
