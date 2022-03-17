<template>
  <div id="data" >
    <h1>Data :</h1>
        <ul>
          <li> Temperature : {{parseFloat(data.temperature)}} °C </li>
          <li> Humidity : {{parseFloat(data.hygrometry)}}  </li>
          <li> Pressure : {{parseFloat(data.pressure)}} Pa </li>
          <li> Luminosity : {{parseFloat(data.brightness)}}  </li>
          <li> Wind_heading : {{parseFloat(data.winddirection).toFixed(2)}}  </li>
          <li> Wind : average : {{parseFloat(data.windvelocity[0]).toFixed(2)}} / min : {{parseFloat(data.windvelocity[1]).toFixed(2)}} / max : {{parseFloat(data.windvelocity[2]).toFixed(2)}}  </li>
          <li> GPS: {{parseFloat(data.gpsposition[0]).toFixed(2)}} / {{parseFloat(data.gpsposition[1]).toFixed(2)}}  </li>
        </ul>
  </div>
</template>

<script>
// import axios from 'axios';
export default {
  name: 'data',
  data () {
    return {
      sonde : "",
      data: {"temperature" : 0 ,
            "hygrometry" : 0,
            "pressure" : 0,
            "brightness" : 0,
            "winddirection" : 0,
            "windvelocity" : 0,
            "gpsposition" : 0,},
    };
  },

  async created(){
    
  },


  methods: {
    
    async updateValues(){
      
 

      let selected_sonde = document.getElementById("sonde").value;
      if (selected_sonde == "localhost"){
        this.sonde = "http://localhost:3000" ; 
      }

      else {
        Object.keys(this.data).forEach(key => {
          this.sonde = '';
          this.data[key] = '';
        });
      }
      try {
        let fetch_name = this.sonde + "/data/";
        for(let measure in this.data){
          fetch_name = fetch_name + measure.toString() + ","
        }
        fetch_name = fetch_name.slice(0, -1);
        console.log(fetch);
        await fetch(fetch_name).then(a => a.json()).then(a => { 
            for (let measure in this.data){
              
              this.data[measure.toString()]=a[measure.toString()].value;
            }
            });
        
      } catch (error) {
        console.log(error);
      }

      setTimeout(this.updateValues, 1000);
    }
  },

  async mounted(){

    this.updateValues();
  }

    
}
</script>

<style lang="scss">
#data {
  list-style-position: inside;
  text-align: center;
  background-color: rgb(214, 239, 245);
}
</style>
