<template>
<div id="app">
  <h2>🌤️ Aplicación del Clima</h2>
  <input v-model="ciudad" placeholder="Ingresa una ciudad" />
  <button @click="buscarClima">Obtener Clima</button>

  <div v-if="clima" class="resultado">
    <h3>Clima en {{ ciudad }}</h3>
    <p>🌡️ Temperatura: {{ clima.temperature }} °C</p>
    <p>💨 Viento: {{ clima.windspeed }} km/h</p>
  </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
name: 'App',
data() {
  return {
    ciudad: '',
    clima: null
  };
},
methods: {
  async buscarClima() {
    try {
      // 1. Buscamos las coordenadas de la ciudad ingresada
      const geoRes = await axios.get(`https://geocoding-api.open-meteo.com/v1/search?name=${this.ciudad}&count=1`);
      const lat = geoRes.data.results[0].latitude;
      const lon = geoRes.data.results[0].longitude;

      // 2. Buscamos el clima usando esas coordenadas
      const climaRes = await axios.get(`https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current_weather=true`);
      
      // 3. Guardamos el resultado para mostrarlo en pantalla
      this.clima = climaRes.data.current_weather;
    } catch (error) {
      alert('No se pudo encontrar la ciudad. Revisa que esté bien escrita.');
    }
  }
}
};
</script>

<style>
#app {
font-family: Arial, sans-serif;
text-align: center;
margin-top: 50px;
}
input {
padding: 10px;
margin-right: 10px;
border-radius: 5px;
border: 1px solid #ccc;
}
button {
padding: 10px 15px;
cursor: pointer;
background-color: #42b983;
color: white;
border: none;
border-radius: 5px;
}
.resultado {
margin-top: 20px;
padding: 20px;
background-color: #f0f8ff;
border-radius: 10px;
display: inline-block;
box-shadow: 0px 4px 6px rgba(0,0,0,0.1);
}
</style>