
<style>
  body {
    overflow: hidden !important;
  }
  div.hex1 {
    background-image: url(./../assets/Hex-Pattern-2.png);
    position: relative;
    background-repeat: repeat;
    height: 100%;
  }
  div.hex2 {
    background-image: url(https://i.imgur.com/iRrLAT3.png);
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
  #image.rotate90 {
    transform: rotate(90deg) translateY(-100%);
    -webkit-transform: rotate(90deg) translateY(-100%);
    -ms-transform: rotate(90deg) translateY(-100%);
}
#image.rotate180 {
    transform: rotate(180deg) translate(-100%,-100%);
    -webkit-transform: rotate(180deg) translate(-100%,-100%);
    -ms-transform: rotate(180deg) translateX(-100%,-100%);
}
#image.rotate270 {
    transform: rotate(270deg) translateX(-100%);
    -webkit-transform: rotate(270deg) translateX(-100%);
    -ms-transform: rotate(270deg) translateX(-100%);
}
</style>

<template>
  <v-app>
    <v-card
      color="grey lighten-4"
      absolute
      dark
      flat
      tile
      >

      <v-toolbar  style="padding-top: 15px" v-show="!hidden">
        <v-slider
          v-model="slider"
          min="2"
          max="20"
          step="0.5"
          ticks>
        </v-slider>
        <v-switch
          v-model="hexSwitch"
          label="Hex Tile">
        </v-switch>
        <v-spacer></v-spacer>
        <v-btn
          icon
          color="indigo"
          @click="rotateImage()"
          >
          <v-icon>mdi-cached</v-icon>
        </v-btn>
        <v-text-field
          v-model="mapImage"
          solo
          label="Image URL"
          clearable>
        </v-text-field>
      </v-toolbar>
    </v-card>
    <div class="grid text">
      <div v-for="tile in this.drawMap" :key="mapGrid.indexOf(tile)">
      {{tile}}
      </div>
      <v-img class="map" :height="mapHeight" :width="mapWidth" :src="mapImage" ></v-img>
      <div
        :style="{'background-size': slider + '%'}"
        :class="hexSwitch ? 'hex1' : 'hex2'" >
        <v-btn
          class="mx-2"
          fab
          small
          dark
          color="indigo"
          style="margin-top: 5px;"
          @click="hidden = !hidden"
          >
          {{ hidden ? 'Show' : 'Hide' }}
        </v-btn>
      </div>
      <div
        :style="{'background-image': 'url('+ mapImage + ');'}"
        class="map" id="map">
      </div>
    </div>
  </v-app>
</template>
<script>
  export default {
    name: 'Grid',
    components: {
    },
    data: () => ({
      rotation: 0,
      hidden: false,
      hexSwitch: true,
      mapImage: '',
      mapGrid: null,
      mapHeight: "100%",
      mapWidth: "100%",
      slider: 2,

    }),
    computed: {},
    created() {
       this.mapGrid = this.drawMap({map_x: 5, map_y: 5});
    },
    methods: {
      drawMap({map_x, map_y}) {
        console.log('#### Initiating Draw Map!');

        const mapGrid = [];

        let mapHeight = mapGrid.length;

        console.log('#### Curr mapHeight: ', mapHeight);

        if(!mapGrid.length) {
          let mapRow = [];

          console.log('x: ', map_x, 'y: ', map_y);

          while(mapGrid.length < map_x) {

            while(mapRow.length < map_y) {
              mapRow.push(this.makeTile(mapGrid.length, mapRow.length + 1))
              mapGrid.push(mapRow)
            }
          }
            console.log(this.mapGrid);

          /*
          #1 [ tile, {id: 1, h: 1, w: 2, clicked: false, etc}, 0, 0, 0],
          [ 0, 0, 0, 0, 0],
          [ 0, 0, 0, 0, 0],
          [ 0, 0, 0, 0, 0],
          [ 0, 0, 0, 0, 0],



          with hexes:

          [ 0, 1, 2, 3, 4, 5, 0, ],
          [ 1, 2, 3, 4, 5, 0, ],
          [ 0, 1, 2, 3, 4, 5, 0, ],
          [ 1, 2, 3, 4, 5, 0, ],
        ]
          */

        }
        return mapGrid;
      },

      makeTile(tile_x, tile_y) {
        const tile = {
          x: tile_x,
          y: tile_y,
          c: false,
        }
        return tile
      },

      rotateImage() {
        // const map = document.getElementById('map');


        // Goal:
        // Create ability to move background image around if too large.
        // Currently using the "cover" property to display "map"

        // Long term:
        // changing the repeated "hex pattern" into
        //individual images that user can interact with

      },
    },
  }
</script>
