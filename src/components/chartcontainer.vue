<template>
    <div class="chartjs">
        <chartjs  v-if="loaded" :chartdata="chartdata" :options="options" />
    </div>
</template>

<script>
  import chartjs from './chartjs.vue'
  export default {
    name: 'chartcontainer',
    components: { chartjs },
    data() {return {
      loaded: false,
      chartdata: null,
      options: {
        responsive: true,
        maintainAspectRatio: false
      }
    }},
    async mounted () {

      console.log(this.loaded);
      this.loaded = false;

      var method = "GET";
      var url = "http://api.openweathermap.org/data/2.5/forecast?id=3096472&APPID=7d0e6193982fab64b6809bed2ad00c0c";
      var xhr = new XMLHttpRequest();
      xhr.open(method, url);
      xhr.onload = function () {
        if (xhr.readyState === xhr.DONE) {
          if (xhr.status === 200) {
            var data = JSON.parse(xhr.response);

            this.chartdata = {
              labels: ['January', 'February'],
              datasets: [
                {
                  label: 'Data test',
                  backgroundColor: '#f87979',
                  data: [40, 20]
                }
              ]
            };

            // this.chartdata = {
            //   labels: data.list.map(listItem => listItem.dt_txt),
            //   datasets: [
            //     {
            //       label: 'Temperature',
            //       backgroundColor: '#f87979',
            //       data: data.list.map(listItem => listItem.main.temp),
            //     }
            //   ]
            // };

            this.loaded = true;
            console.log(this.chartdata);
            console.log(this.loaded);
          }else{
            console.log("Retrieve API data failed!");
          }
        }
      };
      xhr.send();
    }
  }

</script>