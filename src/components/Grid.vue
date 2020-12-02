
<style>
  body {
    overflow:hidden;
  }
  div.hex1 {
    background-image: url(./../assets/Hex-Pattern-2.png);
    position: relative;
    background-repeat: repeat;
    height: 100%;
  }
  div.hex2 {
    background-image: url(./../assets/Hex-Pattern.png);
    background-repeat: repeat;
    position: relative;
    height: 100%;
   
  }
  div.grid {
    width: 100vw;
    height: 100%;
    margin: auto;
    position: relative;
    padding: 0;
  }
  div.map {
    width: 100vw;
    height: 100%;
    position: absolute;
    background-size: contain;
  }
</style>

<template>
  <v-app>
    <v-card
      color="grey lighten-4"
      absolute
      dark
      flat
      tile>
      <v-toolbar  style="padding-top: 15px">
        <v-slider
          v-model="slider"
          min="2"
          max="20"
          step="0.5"
          ticks>
        </v-slider>
        <v-switch
          v-model="switch1"
          label="Hex Tile">
        </v-switch> 
        <v-spacer></v-spacer>
        <v-text-field
          v-model="mapImage"
          solo
          label="Image URL"
          clearable>
        </v-text-field>
      </v-toolbar>
    </v-card>   
    <div class="grid">
      <v-img class="map" :src="mapImage" ></v-img>
      <div 
        :style="{'background-size': slider + '%'}" 
        :class="switch1 ? 'hex1' : 'hex2'" >
      </div>
      <div 
        :style="{'background-image': 'url('+ mapImage + ');'}" 
        class="map">
      </div>
    </div>
  </v-app>
</template>
<script>
// const hexImage = require('@/assets/Hex-Pattern-2.png');
// style="background-image: url(/img/Hex-Pattern-2.cefb0150.png)"
  export default {
    name: 'Grid',
    components: {
    },
    data: () => ({
      switch1:true,
      mapImage: '',
      // hexImage: "./../assets/Hex-Pattern-2.png",
      slider: 2,
      
    }), 
    computed: {

    }
  }
</script>
