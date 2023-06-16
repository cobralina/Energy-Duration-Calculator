<template>
    <div v-if=dataLoaded class="gen_content">
    <h1>Master Calculator</h1>
    <p>
      Startzeitpunkt eingeben:<br> <small>(Formatierung beachten, dd.mm. hh:00)</small>
      </p>
      <form> 
 <input class="input_generator" v-model="startDate" id="startDate" >
 <br><br>
  <input type="radio" id="threedays" value="threedays" v-model="dieselGen" />
<label for="threedays">Dieselgenerator für 3 Tage</label>
<br>
<input type="radio" id="fivedays" value="fivedays" v-model="dieselGen" />
<label for="fivedays">Dieselgenerator für 5 Tage</label>
<br>
<input type="radio" id="sevendays" value="sevendays" v-model="dieselGen" />
<label for="sevendays">Dieselgenerator für 7 Tage</label>
<br><br>
  <button class="gen_button" v-on:click.prevent="getIndex">Abschicken</button>
</form>
 <br><br>  
   
   
    <div v-if=hasDate class="gen_sourcecode">
      <h2>Ergebnis: </h2>
    Die Energie mit dem Dieseltank reicht bis <b>{{ maxDate }}</b>. Das sind {{ difDays }} Tage und {{ restHours }} Stunden.<br>
    Das sind insgesamt {{ difHours }} Stunden.
    <br><br>
    Der 7-Tage-Mittelwert beträgt {{ difDaysMedian }} Tage, {{ restHoursMedian }} Stunden und {{ restMinutesMedian }} Minuten.<br>
    Das sind insgesamt {{ niceHoursMedian }} Stunden.
    <br><br>
   
   
    <button class="gen_button" v-on:click.prevent="reloadCalculator">Rechner neu laden</button>
 </div>
</div>
<div v-else class="gen_content"><h1>Master Calculator</h1>
    <p>
      Welche Daten sollen geladen werden?<br> 
    </p>
    <form> 
 
  <input type="radio" id="v1" value="v1" v-model="whichFile" />
<label for="v1">Version 1</label>
<br>
<input type="radio" id="v2" value="v2" v-model="whichFile" />
<label for="v2">Version 2</label>
<br>
<input type="radio" id="v3" value="v3" v-model="whichFile" />
<label for="v3">Version 3</label>
<br>
<input type="radio" id="v4" value="v4" v-model="whichFile" />
<label for="v4">Version 4</label>
<br><br>
  <button class="gen_button" v-on:click.prevent="getFileData">Auswählen</button>
</form>
<div v-if="dataChoosen"><br><br>Lädt Daten ... <br></div>
</div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data () {
    return {
      energyData:[],
      dropdownData: [],
      hasDate: false,
      dataLoaded: false,
      dataChoosen: false,
      startDate: "",
      whichFile:"",
      finalTimestamp: "",
      startIndex:0,
      maxDate:"",
      difHours:"",
      difHours1:"",
      difHours2:"",
      difHours3:"",
      difHours4:"",
      difHours5:"",
      difHours6:"",
      difHoursMedian:"",
      difDaysMedian:"",
      restHoursMedian:"",
      restMinutesMedian:"",
      difDays:"",
      restHours:"",
      dieselGen:"",
      niceHoursMedian:""
    }
  },
  methods: {

    reloadCalculator: function () {
      this.hasDate= false;
      this.dataLoaded= false;
      this.dataChoosen= false;
    },
     getEnergyData: function (_url) {
        
        let self = this;

        this.$papa.parse(_url, {
            download: true,
            header: true,
            complete: function(results) {              
                    for (var j = 0; j < results.data.length; j++) {
                    let dataObject = results.data[j]
                    
                    self.energyData.push(dataObject);
                    self.dropdownData.push(dataObject.Zeit);
                    
                  }

                  self.dataLoaded=true;

            }
        });
        
    },
    getFileData: function () {

      let dataUrl = "";

      if (this.whichFile=="v1")
        {
          dataUrl = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRJiBHkbWnEJRC67JL98C5Znzhu0xJSPK9d6gfH0Q9zBdynSsdNLQ1Wp5OgYgojvAqCY6ktOyzX1fbP/pub?gid=754900931&single=true&output=csv";
        }
        if (this.whichFile=="v2")
        {
          dataUrl = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRJiBHkbWnEJRC67JL98C5Znzhu0xJSPK9d6gfH0Q9zBdynSsdNLQ1Wp5OgYgojvAqCY6ktOyzX1fbP/pub?gid=1294138184&single=true&output=csv";
        }
        if (this.whichFile=="v3")
        {
          dataUrl = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRJiBHkbWnEJRC67JL98C5Znzhu0xJSPK9d6gfH0Q9zBdynSsdNLQ1Wp5OgYgojvAqCY6ktOyzX1fbP/pub?gid=40426423&single=true&output=csv";
        }
        if (this.whichFile=="v4")
        {
          dataUrl = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRJiBHkbWnEJRC67JL98C5Znzhu0xJSPK9d6gfH0Q9zBdynSsdNLQ1Wp5OgYgojvAqCY6ktOyzX1fbP/pub?gid=455017333&single=true&output=csv";
        }

        this.dataChoosen = true;
        this.getEnergyData(dataUrl);
      

    },
    getIndex: function () {
           

           for (var i=0; i < this.energyData.length; i++)
           {
            if (this.energyData[i].Zeit == this.startDate)
            {
              this.startIndex = i;
              this.getTimestamp(this.startIndex);
            }
           }
        },
    
    getTimestamp: function (_startIndex) {
      let energySum = 0;
      let energySum1 = 0;
      let energySum2 = 0;
      let energySum3 = 0;
      let energySum4 = 0;
      let energySum5 = 0;
      let energySum6 = 0;
      let dieselGenThree = 2362.44;
      let dieselGenFive = 3937.40;
      let dieselGenSeven = 5512.36;
      let maxSum = 0;

      
      if (this.dieselGen=="sevendays")
        {
          maxSum = dieselGenSeven;
        }
      if (this.dieselGen=="threedays")
        {
          maxSum = dieselGenThree;
        }
      if (this.dieselGen=="fivedays")
        {
          maxSum = dieselGenFive;
        }

      for (var i=_startIndex; i < this.energyData.length; i++)
           {
            let addEnergy = this.energyData[i].Netzbezug.replace(",",".");
            energySum = energySum + parseFloat(addEnergy);
            
             if (energySum >= maxSum){

              for (var j=_startIndex-3; j < this.energyData.length; j++)
                {           
                  let addEnergy1 = this.energyData[j].Netzbezug.replace(",",".");
                  energySum1 = energySum1 + parseFloat(addEnergy1);
                  if (energySum1 >= maxSum){
                    this.difHours1 = this.energyData[j].Stunden - this.energyData[_startIndex-3].Stunden
                    break;
                  }
                } 

                for (var k=_startIndex-2; k < this.energyData.length; k++)
                {           
                  let addEnergy2 = this.energyData[j].Netzbezug.replace(",",".");
                  energySum2 = energySum2 + parseFloat(addEnergy2);
                  if (energySum2 >= maxSum){
                    this.difHours2 = this.energyData[j].Stunden - this.energyData[_startIndex-2].Stunden
                    break;
                  }
                } 

                for (var l=_startIndex-1; l < this.energyData.length; l++)
                {           
                  let addEnergy3 = this.energyData[l].Netzbezug.replace(",",".");
                  energySum3 = energySum3 + parseFloat(addEnergy3);
                  if (energySum3 >= maxSum){
                    this.difHours3 = this.energyData[l].Stunden - this.energyData[_startIndex-1].Stunden
                    break;
                  }
                } 

                for (var m=_startIndex+1; m < this.energyData.length; m++)
                {           
                  let addEnergy4 = this.energyData[m].Netzbezug.replace(",",".");
                  energySum4 = energySum4 + parseFloat(addEnergy4);
                  if (energySum4 >= maxSum){
                    this.difHours4 = this.energyData[m].Stunden - this.energyData[_startIndex+1].Stunden
                    break;
                  }
                } 

                for (var n=_startIndex+2; n < this.energyData.length; n++)
                {           
                  let addEnergy5 = this.energyData[n].Netzbezug.replace(",",".");
                  energySum5 = energySum5 + parseFloat(addEnergy5);
                  if (energySum5 >= maxSum){
                    this.difHours5 = this.energyData[n].Stunden - this.energyData[_startIndex+2].Stunden
                    break;
                  }
                } 

                for (var o=_startIndex+3; o < this.energyData.length; o++)
                {           
                  let addEnergy6 = this.energyData[o].Netzbezug.replace(",",".");
                  energySum6 = energySum6 + parseFloat(addEnergy6);
                  if (energySum6 >= maxSum){
                    this.difHours6 = this.energyData[o].Stunden - this.energyData[_startIndex+3].Stunden
                    break;
                  }
                } 

              this.hasDate = true;
              this.maxDate = this.energyData[i].Zeit
              this.difHours = this.energyData[i].Stunden - this.energyData[_startIndex].Stunden
              this.difDays = parseInt(this.difHours/24);
              this.restHours = this.difHours % 24;
              
              this.difHoursMedian = (this.difHours1 + this.difHours2 + this.difHours3 + this.difHours4 + this.difHours5 + this.difHours6 + this.difHours) / 7
              this.difDaysMedian = parseInt(this.difHoursMedian/24);
              this.restHoursMedian = parseInt(this.difHoursMedian % 24);
              let kruecke = (this.difHoursMedian % 24) - this.restHoursMedian 
              this.restMinutesMedian = parseInt(kruecke*60);
              this.niceHoursMedian = this.difHoursMedian.toFixed(2).replace(".",",");
             
              break;
            }
    
           }

    }
  },
  mounted () {

  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import './assets/styles.css';
</style>


