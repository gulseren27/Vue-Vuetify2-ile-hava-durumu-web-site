<template>
  
  <div class="weathers">
    
    
  <v-container class="cerceve " >
    <v-row class="text-center my-10" >
     

      <v-col cols="12" sm="6" offset-sm="3" >
            <v-card class="pa-4 " elevation="5">    
              <h2 class="mb-4">Hava Durumu</h2>
              <v-text-field
                v-model="location"
                label="Şehir"
                outlined
                dense
                rounded 
                @input="getWeatherData()"
              ></v-text-field>

              

             
            

              <div class="grey--text mb-2" v-if="weatherData && weatherData.main"> Feels Like {{ weatherData.main.feels_like }}°C</div>

            
              <div class="circle-icon">
                <div class="içerdeki_ikon">   
                  <!-- ikonu circle ın üstüne getirebilmek için ayrı bir div in içine aldım -->
                  <v-icon class="bulut_ikon" color="black" size="65">mdi mdi-clouds</v-icon>
                  <span class="inner-text" v-if="weatherData && weatherData.main">{{ weatherData.main.temp }}°C</span>
                </div>
              </div>


              <div class="grey--text mt-3" v-if="weatherData && weatherData.main"> {{ weatherData.weather[0].description }} </div>

              
              <v-divider class="my-4"></v-divider>

            
            <v-row  class="ikonlar " v-if="weatherData && weatherData.main">

              <v-icon class="ikon " color="blue-grey darken-2 " size="30">
                  mdi mdi-weather-windy 
                  </v-icon>
                  {{ weatherData.wind.speed }}
                  <v-icon class="ikon" color="blue-grey darken-2" size="30">
                    mdi mdi-weather-rainy
                  </v-icon>
                  %{{weatherData.rain}}
                  <v-icon class="ikon" color="blue-grey darken-2" size="30">
                  mdi mdi-weather-cloudy 
                </v-icon> 
                 %{{weatherData.clouds.all}}
            
            </v-row>
            </v-card>
          </v-col>
        </v-row>
  </v-container>
  
  </div> 
</template> 


<script>


  import axios from 'axios';
  

  


  export default {
    name: 'HelloWorld',
    components: {
      
      
    
  },

    data: () => ({
      location: "",
       weatherData: 
       {
       main: {
         temp: "", 
         feels_like: "",
       },
       weather: [
         {
           description: " ",
         },
       ],
       wind: {
         speed: null,
       },
       rain: "",
       clouds: {
         all: null,
       },
     },
      
 }),
    methods: {


       async getWeatherData() {

        const apiKey = '03f87ac1004723a289753f86ffc5cd4e'
        const city = this.location; 
        const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}`;


        try {
        
        const response = await axios.get(apiUrl);
        this.weatherData = response.data;
         // Sıcaklığı Celsius'a dönüştür
         if (this.weatherData.main && this.weatherData.main.temp !== null) {
        this.weatherData.main.temp = (this.weatherData.main.temp - 273.15).toFixed(1);
        this.weatherData.main.feels_like = (this.weatherData.main.feels_like - 273.15).toFixed(1);
      }
       } 
       catch (error) {
         console.error(error);
       }
       
     },
  },
}
  
</script>


<style >

.weathers {
  background: url('../assets/bulut.png') ;
  background-size: cover;
  width: 100%;
  height: 100%;

}


.bulut_ikon{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-130%, -75%);
  
}

.circle-icon{
  width: 160px; /* Daire ikonunun boyutu */
  height: 150px;
  margin-left: 188px;
  border-radius: 50%; /* Daire şekli */
  background-color: lightgray; /* Arka plan rengi */
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 45px; /* Metin boyutu */
  font-weight:200;
  color:black; /* Metin rengi */
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
  
  
}
.içerdeki_ikon{
  position: relative;
}
.inner-text{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.ikonlar {
    margin-top: 10px; 
    display: flex;
    align-items: center;
    /* justify-content: space-around; */
  }

  .ikon {
    margin-left: 100px; /* İkonlar arasındaki boşluk */
  }



</style>

