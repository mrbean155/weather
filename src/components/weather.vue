<template>
  <div class="row">
      <div class="col-md-4">
        <div class="card">
          <div class="card-header">
            <h3>Weather in {{ city }}, PL</h3>
          </div>
          <div class="card-body">
            <div id="weatherApp">
              <div class="weather-details">
                  <span class="time">{{ setTime() }}</span> &amp;
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
            <chartcontainer/>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import chartcontainer from './chartcontainer'

export default {
  name: 'weather',
  data() {return {
      temp: 10,
      time: new Date(),
      city: 'Katowice'
  }},
  components: {
    chartcontainer
  },
  methods: {
    setTime: function() {
      var timeAtLoad = this.time;
      return timeAtLoad.toLocaleTimeString([], {timeZone: 'Europe/Warsaw', hour: '2-digit', minute:'2-digit', hour12: true})
    },
    setLocation: function() {
      return 111;
    },
    getData: function() {

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
