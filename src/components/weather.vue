<template>
  <div class="row">
      <div class="col-md-4">
        <div class="card">
          <div class="card-header">
            <h3>Weather in Katowice, PL</h3>
          </div>
          <div class="card-body">
            <div id="weatherApp">
              <div class="weather-details">
                  <span class="temp">{{ temp }}</span>&deg;C
              </div>
              <div class="btn-group">
                <button @click="getData" type="button" class="btn btn-info">Get Data</button>
              </div>
            </div>
          </div>
        </div>
    </div>
    <div class="col-md-8">
      <div class="card">
        <div class="card-body">
          <div id="chart">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'weather',
  data() {return {
      temp: 10,
      time: new Date(),
  }},
  methods: {
    getData: function() {
      var method = "GET";
      var url = "http://api.openweathermap.org/data/2.5/forecast?id=3096472&APPID=7d0e6193982fab64b6809bed2ad00c0c";
      var xhr = new XMLHttpRequest();
      xhr.open(method, url);
      xhr.onload = function () {
        if (xhr.readyState === xhr.DONE) {
          if (xhr.status === 200) {
            var data = JSON.parse(xhr.response);
            console.log(data);
          }else{
            console.log("Retrieve API data failed!");
          }
        }
      };
      xhr.send();
    }
  }
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
</style>
