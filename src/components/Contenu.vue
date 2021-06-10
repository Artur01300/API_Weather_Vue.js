<template>
  <div class="changeImg" :class="typeof weather.main != 'undefined' && weather.main.temp > 18 ? 'warm' : ''">
    <main>
      <!-- primier alertOn de v-binde (v-bind:alertOn="alertOn") on a pris depuis propriété data -->
    <alert v-bind:alertOn="alertOn" v-bind:alertOff="alertOff"></alert>
      <div class="search-box">
        <!--v-model="query":  récupère l'inpute-->
        <!--  @keypress="fetchWeather": permet d'avoir le fonction d'entrer -->
        <input type="text" 
          class="search-bar"
          placeholder="Recherche..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <!-- si les données du weather.main sont manquantes on afficher ce contenue -->
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dataBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
          <div class="weather">{{weather.weather[0].description}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

import ModaleAlert from './ModaleAlert'

export default {
  name: 'Contenu',
  components: {
    // on done un nom pour l'utilisation ('alert') à partice le ModelAlert
    'alert': ModaleAlert

  },
  data (){
    return {
      //j'ajoute la clé proposé par le site de cet API (https://home.openweathermap.org/api_keys)
      api_key: '6eb1e4c44e0f04b72864db80d0f37618',
       //j'ajoute l'url proposé par le site de cet API (https://openweathermap.org/current), voir "API call"
      url_base: 'http://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      alertOn: false
    }
  },
  methods: {
    fetchWeather(e){
      /*
        on vérifi si l'utilisateur a appuié sur le bouton d'entrer pour la recherche des pays
        (une fois le nom de pays écrit). Si oui alors on exécute le code
      */
      if(e.key == "Enter"){
        //dans le fetch on implément l'instuction données par ce site internet: (https://openweathermap.org/current)
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&lang=fr&APPID=${this.api_key}`)
        .then(res => {
          if(res.status === 404){
            this.alertOn = true;
          }
          return res.json();
        }).then(this.setResults);
      }
    },
    alertOff(){
      this.alertOn = !this.alertOn
    },
    setResults (results){
      this.weather = results;
      console.log('results', this.weather)
    },
    dataBuilder(){
      let d = new Date();
      let months = [
        "Janvier", "Février", "Mars", "Avril", "Mai", "Juin", 
        "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre", 
      ];
      let days = [
        "Dimanche", "Lundi", "Mardi", "Mecredi", "Jeudi", "Vendredi", "Samedi"
      ];

      let day = days[d.getDay()];
      let data = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${data} ${month} ${year}`;
    }
  }
};
</script>

<style>

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.changeImg{
  background-image: url('../assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;

}
.changeImg.warm {
  background-image: url('../assets/warm-bg.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}


.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0 , 0, 0.25);
}

.location-box .date{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather{
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

</style>
