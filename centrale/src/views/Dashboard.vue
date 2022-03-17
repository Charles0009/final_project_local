<template>
 <div id="history">
    <h1>History :
    </h1>
    <p>Data:
    <select v-model="selected" @change="getHistory">
      <option disabled value="">Choisissez</option>
      <option >temperature</option>
      <option >hygrometry</option>
      <option >pressure</option>
      <option >brightness</option>
      <option >winddirection</option>
      <option >windvelocity</option>
    </select>


    Period:
      <select v-model='time_period' @change="getHistory">
        <option :value="60000">1 minute</option>
        <option :value="3600000">1 hour</option>
        <option :value="86400000">1 day</option>
        <option :value="604800000">1 week</option>
        <option :value="2592000000">1 month</option>
      </select>
    </p>
    <line-chart :dates="dates" :selected="selected" :values="values"></line-chart>
  </div>
</template>

<script>
import LineChart from '@/views/Line'
export default {
  components: {
    LineChart
  },
  data () {
    return {
      selected:'temperature',
      time_period: 60000,
      dates: [],
      values: [],
      
    }
  },
  methods: {

    getHistory: function(){

      let stop = new Date(Date.now());
      let start = new Date(stop.getTime() - this.time_period);

      this.loading = true;

      // Fetching the history from the API
      fetch('http://' + "localhost" + ':3000/data/'+ this.selected.toString()+ '/'+ start.toISOString() + ',' + stop.toISOString()).then(result=>{
        return result.json();
      }).then(result=>{
 
        let measurements_array = result;
        

        this.dates = measurements_array.map(measure=>measure[this.selected].date);
        this.values = measurements_array.map(measure=>measure[this.selected].value[0]);
        

        for(let i = 0; i < this.dates.length; i++){
          let date = new Date(this.dates[i]);
          let finalString = "";
          finalString += date.getDate() + "/" + (date.getMonth()+1) + "/" + date.getFullYear();
          finalString += " ";
          finalString += date.getHours();
          finalString += ":";
          finalString += date.getMinutes();
          this.dates[i] = finalString;
        }

      }).catch(()=>{
        
        this.dates = [];
        this.values = [];
        
      });
    }
  },

  mounted: function() {
    
    this.getHistory();
  }


}
</script>