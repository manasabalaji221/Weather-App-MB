<template>
  <div id="app" >
    <main>
      <h1> Weather Application</h1>
      <br />
      <div class="search-box">
        <h3># Search for weather at current location:</h3>
        <br />
        <button type="button" class="buttons" @click="getInitWeather">Current City</button>
        <br />
        <br />
        <h3># Search for weather at any location: </h3>
        <div id="" v-if="city !='undefined'"> </div>
        <input type="text"
               id="cityid"
               class="search-bar"
               placeholder="City Name"
               v-model="query"            
               />
        <br />

        <button type="button" class="buttons" @click="fetchCurrent">Current</button>
        <button type="button" class="buttons" @click="fetchHourly">Hourly</button>
        <button type="button" class="buttons" @click="fetchDaily">Daily</button>
        <br />
      </div>

      <div class="weather-info" v-if="typeof weather.main != 'undefined'">
        <div class="row">
          <br />

          <div class="column" v-if="flag == 1">
            <h3>Current Update:</h3>
            <div class="location-box">
              
              <br />
              <div><b>Location: </b>{{weather.name}}, {{ weather.sys.country }}</div>
              <div><b>Date: </b>{{ dateBuilder() }}</div>
            </div>

            <div class="weather-box">
              <div><b>Temp: </b>{{ Math.round(weather.main.temp) }}°f</div>
              <div><b>Min Temp: </b>{{ Math.round(weather.main.temp_min) }}°f</div>
              <div><b>Max Temp: </b>{{ Math.round(weather.main.temp_max) }}°f</div>

              <div><b>Feels like: </b>{{ Math.round(weather.main.feels_like) }}°f</div>
              <div><b>Weather: </b>{{ weather.weather[0].main }}</div>
              <div><b>Wind Speed: </b>{{ Math.round(weather.wind.speed) }}</div>
              <br />
              <b>JSON Data:</b>
              {{ weather }}
            </div>
          </div>

          <div class="column" v-if="flag == 2">
            <h3>Hourly Update:</h3>
            <div class="location-box">
              <br />
              <div><b>Location: </b>{{weather.name}}, {{ weather.sys.country }}</div>
              <div><b>Date: </b>{{ dateBuilder() }}</div>
            </div>

            <div class="weather-box">
              <div><b>Temp: </b>{{ Math.round(weather.main.temp) }}°f</div>
              <div><b>Min Temp: </b>{{ Math.round(weather.main.temp_min) }}°f</div>
              <div><b>Max Temp: </b>{{ Math.round(weather.main.temp_max) }}°f</div>

              <div><b>Feels like: </b>{{ Math.round(weather.main.feels_like) }}°f</div>
              <div><b>Weather: </b>{{ weather.weather[0].main }}</div>
              <div><b>Wind Speed: </b>{{ Math.round(weather.wind.speed) }}</div>
              <br />
              <b>JSON Data:</b>
              {{ weather }}
            </div>
          </div>

          <div class="column" v-if="flag == 3">
            <h3>Daily Update:</h3>
            <div class="location-box">
              {{ weather }}
              <br />
              <div><b>Location: </b>{{weather.name}}, {{ weather.sys.country }}</div>
              <div><b>Date: </b>{{ dateBuilder() }}</div>
            </div>

            <div class="weather-box">
              <div><b>Temp: </b>{{ Math.round(weather.main.temp) }}°f</div>
              <div><b>Min Temp: </b>{{ Math.round(weather.main.temp_min) }}°f</div>
              <div><b>Max Temp: </b>{{ Math.round(weather.main.temp_max) }}°f</div>

              <div><b>Feels like: </b>{{ Math.round(weather.main.feels_like) }}°f</div>
              <div><b>Weather: </b>{{ weather.weather[0].main }}</div>
              <div><b>Wind Speed: </b>{{ Math.round(weather.wind.speed) }}</div>
              <br />
              <b>JSON Data:</b>
              {{ weather }}
            </div>
          </div>
        </div>

      </div>

    </main>
  </div>
</template>

<script>
 
  export default {
    name: 'app',
    data() {
      return {
        api_key: '612a2ea7889722e9eafed2470646dac8',
        url_base: 'https://api.openweathermap.org/data/2.5/',
        query: '',
        flag: 0,
        cflag: 0,
        weather: {},
        city: '',
        coord: "",
        lat: 0,
        lng: 0
      }
    },
    mounted: function () {
      /*this.getInitWeather()*/
      if (localStorage.query) this.query = localStorage.query;
      if (localStorage.query) document.getElementById("cityid").value = localStorage.query;
    },
    watch: {
      query(q) {
        localStorage.query = q;
      }
    },
    methods: {
      fetchWeather(e) {
        if (e.key == "Enter") {
          fetch(`${this.url_base}weather?q=${this.query}&units=imperial&APPID=${this.api_key}`)
            .then(res => {
              return res.json();
            }).then(this.setWeather);

        }
      },
      fetchCurrent() {
        this.flag = 1;        
        var query = document.getElementById("cityid").value;
        console.log(`${query}`);
        fetch(`${this.url_base}weather?q=${query}&units=imperial&exclude=current&APPID=${this.api_key}`)
            .then(res => {
              return res.json();
          }).then(this.setWeather);
        document.getElementById("cityid").value = query;
      },
      fetchHourly() {
        this.flag = 2;
        var query = document.getElementById("cityid").value;
        fetch(`${this.url_base}weather?q=${query}&units=imperial&exclude=hourly&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setWeather);
        document.getElementById("cityid").value = query;
      },
      fetchDaily() {
        this.flag = 3;
        var query = document.getElementById("cityid").value;
        fetch(`${this.url_base}weather?q=${query}&units=imperial&exclude=daily&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setWeather);
        document.getElementById("cityid").value = query;
      },
      setWeather(results) {
        this.weather = results;
      },
      dateBuilder() {
        let d = new Date();
        let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
        let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
        let day = days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();
        return `${day} ${date} ${month} ${year}`;
      },
      getInitWeather: function() {
      var options = {
        enableHighAccuracy: true,
        timeout: 5000,
        maximumAge: 0
      };

      function success(pos) {
        var crd = pos.coords;
        var lat = crd.latitude.toString();
        var lng = crd.longitude.toString();
        var coordinates = [lat, lng];
        console.log(`Latitude: ${lat}, Longitude: ${lng}`);
        console.log(`${coordinates}`)


        var xhr = new XMLHttpRequest();
        var lat = coordinates[0];
        var lng = coordinates[1]; 

        // Paste your LocationIQ token below. 
        xhr.open('GET', " https://us1.locationiq.com/v1/reverse.php?key=pk.21692d96e8830b0132e48b7f80314487&lat=" +
          lat + "&lon=" + lng + "&format=json", true);
        xhr.send();
        xhr.onreadystatechange = processRequest;
        xhr.addEventListener("readystatechange", processRequest, false);

        function processRequest(e) {
          if (xhr.readyState == 4 && xhr.status == 200) {
            var response = JSON.parse(xhr.responseText);
            var city = response.address.city;
            this.city = city;
            console.log(`${this.city}`);
            this.cflag = 1;
            document.getElementById("cityid").value = city;
                     
            return `${city}`;

          }
        }

      }

      function error(err) {
        console.warn(`ERROR(${err.code}): ${err.message}`);
      }

      navigator.geolocation.getCurrentPosition(success, error, options);
      },
      getCity() {
        
      } 
    }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: 'montserrat', sans-serif;
  }

  #app {
    background-color: aquamarine;
    background-size: cover;
  }

  

  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  }

  

    .search-box .search-bar {
      display: block;
      width: 30%;
      padding: 15px;
      margin-top:20px;
      color: #313131;
      font-size: 20px;
      appearance: none;
      border: none;
      outline: none;
      background: none;
      box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.5);
      transition: 0.4s;
    }



  .location-box .location {
    color: #FFF;
    font-size: 32px;
    font-weight: 500;
    text-align: left;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date {
    color: #FFF;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: left;
  }

  .weather-info {
    text-align: left;
  }

  .buttons {
    padding: 5px;
    background-color: mediumaquamarine;
  }

  .column {
    float: left;
    width: 33.33%;
  }

  /* Clear floats after the columns */
  .row:after {
    content: "";
    display: table;
    clear: both;
  }
  
  
</style>

