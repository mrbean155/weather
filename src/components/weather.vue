<template>
  <div class="row">
    <div class="col-md-3">
      <div v-if="info_loaded" class="card">
        <div class="card-header">
          <h3>Weather in {{ city }}, {{ country }}</h3>
        </div>
        <div class="card-body">
          <div class="weather-details">
            <h4>
              <img :src=" 'http://openweathermap.org/img/wn/' + icon + '@2x.png'" width="60px" height="60px" alt="">
              <b>{{ temperature }}&deg;C</b>
            </h4>
            <h4>{{ description }}</h4>

            <table class="table">
              <tr>
                <td>Humidity</td>
                <td>{{ humidity }} %</td>
              </tr>
              <tr>
                <td>Pressure</td>
                <td>{{ pressure }} hpa</td>
              </tr>
              <tr>
              <tr>
                <td>Wind</td>
                <td>{{ wind }} km/h</td>
              </tr>
            </table>
          </div>
        </div>
      </div>
    </div>
    <div class="col-md-9">
      <div class="card">
        <div class="card-body">
          <div v-if="loaded" class="chartjs">
            <chartjs :chartdata="temp_data" :options="options" />
            <chartjs :chartdata="humidity_data" :options="options" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import chartjs from './chartjs.vue';
  export default {
    name: 'weather',
    data() {return {
      temperature: 10,
      kelvin_base: 273.15,
      city: null,
      country: null,
      icon: null,

      info_loaded: false,
      loaded: false,

      temp_data: null,
      humidity_data: null,

      humidity: null,
      pressure: null,
      wind: null,
      description: 11,

      options: {
        responsive: true,
        maintainAspectRatio: false
      }
    }},
    components: {
      chartjs
    },
    methods: {
      getInfo: function() {
        this.info_loaded = false;
        var method = "GET";
        var url = "http://api.openweathermap.org/data/2.5/weather?id=3096472&APPID=7d0e6193982fab64b6809bed2ad00c0c";
        var xhr = new XMLHttpRequest();
        xhr.open(method, url);
        xhr.onload = () => {
          if (xhr.readyState === xhr.DONE) {
            if (xhr.status === 200) {
              var data = JSON.parse(xhr.response);
              // console.log(data);

              this.city = data.name;
              this.country = data.sys.country;
              this.icon = data.weather[0].icon;

              this.temperature = Number((data.main.temp - this.kelvin_base).toFixed(1));
              this.humidity = data.main.humidity;
              this.pressure = data.main.pressure;
              this.wind = data.wind.speed;
              this.description = data.weather[0].description;

              this.info_loaded = true;
            }else{
              console.log("Retrieve location data failed!");
            }
          }
        };

        xhr.send();

      },
      getData: function() {
        this.loaded = false;
        var method = "GET";
        var url = "http://api.openweathermap.org/data/2.5/forecast?id=3096472&APPID=7d0e6193982fab64b6809bed2ad00c0c";
        var xhr = new XMLHttpRequest();
        xhr.open(method, url);
        xhr.onload = () => {
          if (xhr.readyState === xhr.DONE) {
            if (xhr.status === 200) {
              var data = JSON.parse(xhr.response);

              this.temp_data = {
                labels: data.list.map(listItem => listItem.dt_txt),
                datasets: [
                  {
                    label: 'Temperature',
                    backgroundColor: '#1780A4',
                    data: data.list.map(listItem => Number((listItem.main.temp -this.kelvin_base).toFixed(1))),
                  }
                ]
              };

              this.humidity_data = {
                labels: data.list.map(listItem => listItem.dt_txt),
                datasets: [
                  {
                    label: 'Humidity',
                    backgroundColor: '#373B8E',
                    data: data.list.map(listItem => listItem.main.humidity),
                  }
                ]
              };

              this.loaded = true;
            }else{
              console.log("Retrieve forecast data failed!");
            }
          }
        };

        xhr.send();

      }
    },
    mounted:function(){
      this.getInfo();
      this.getData();
      setInterval(() => {
        this.getInfo();
        this.getData();
      }, 20000)
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
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
  table td {
    text-align: left;
  }
</style>
