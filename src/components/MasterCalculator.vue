<template>
  <div class="gen_content">
    <h1>Master Calculator</h1>
    <p>
      Startzeitpunkt wählen:
      </p>
      <form> 
  <input class="input_generator" v-model="startDate" id="startDate" >
  <input type="radio" id="threedays" value="threedays" v-model="dieselGen" />
<label for="threedays">Dieselgenerator für 3 Tage</label>

<input type="radio" id="sevendays" value="sevendays" v-model="dieselGen" />
<label for="sevendays">Dieselgenerator für 7 Tage</label>
  <button class="gen_button" v-on:click.prevent="generateTime">Abschicken</button>
</form>
   
   
    <h2>Ergebnis </h2>
    <div class="gen_sourcecode">
    Ergebnis
 </div>
      
  </div>

</template>

<script>
export default {
  name: 'MasterCalculator',
  props: {
    
  },
  data() {
        return {
            hasChoosenDate: false,
            startDate: "",
            listDates: [],
            listEnergy: [],
            finalTimestamp: ""
       }     
    },
    
    methods: {
        generateTime: function () {
           this.hasChoosenDate = true;
           console.log("listEnergy: "+this.listEnergy);
           console.log(this.startDate);
            
        }
    },
    mounted() {
        
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import '../assets/styles.css';
</style>
