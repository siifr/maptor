
<style>
  body {
    overflow:hidden;
  }
  header {
    z-index: 99999;
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
  svg.hex-grid {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
  .hex-tile {
    fill: rgba(255,255,255, 0.35);
    stroke: rgba(0, 0, 0, 0.35);
    stroke-width: 1;
  }
  .hex-tile.alt {
    fill: rgba(76, 175, 80, 0.35);
  }
  .hex-label {
    font-size: 20px;
    fill: #ffffff;
    text-anchor: middle;
    dominant-baseline: middle;
    pointer-events: none;
    user-select: none;
    font-weight: bold;
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
      :style="{ display: headerHidden ? 'none' : 'block' }">
      <v-toolbar  style="padding-top: 15px">
        <v-slider
          v-model="hexRadius"
          min="10"
          max="140"
          step="5"
          ticks>
        </v-slider>
        <v-slider
          v-model="imageScale"
          min="10"
          max="200"
          step="10"
          ticks>
        </v-slider>
        <v-slider
          v-model="imageRotation"
          min="0"
          max="360"
          step="5"
          ticks>
        </v-slider>

        <v-switch
          v-model="switch1"
          :label="switch1 ? 'Night Mode' : 'Day Mode'">
        </v-switch>
        <v-spacer></v-spacer>
        <v-text-field
          v-model="mapImage"
          solo
          label="Image URL"
          clearable>

        </v-text-field>
        <v-btn style="top: -15px;" href="https://modernarmor.worldoftanks.com/en/cms/guides/maps/" target="_blank">
            Maps
        </v-btn>
      </v-toolbar>
    </v-card>
    <v-btn
        fixed
        top
        right
        fab
        icon
        @click="headerHidden = !headerHidden"
        :style="headerHidden ? { top: '0' } : { top: '75px' }">
      <v-icon>{{ headerHidden ? 'menu' : 'hide' }}</v-icon>
    </v-btn>
    <div class="grid">
      <div
        v-if="mapImage"
        class="map"
        :style="{
          backgroundImage: 'url(' + mapImage + ')',
          backgroundSize: imageScale + '%',
          backgroundPosition: 'center',
          transform: 'rotate(' + imageRotation + 'deg)',
          transformOrigin: 'center',
        }">
      </div>
      <svg class="hex-grid">
        <path
          v-for="hex in hexes"
          :key="hex.id"
          class="hex-tile"
          :class="{ alt: !switch1 }"
          :d="hex.d">
        </path>
        <text
          v-for="hex in hexes"
          :key="hex.id"
          class="hex-label"
          :x="hex.cx"
          :y="hex.cy"
          :style="{ fontSize: hexFontSize + 'px' }">
          {{ hex.label }}
        </text>
      </svg>
    </div>

  </v-app>
</template>
<script>
  export default {
    name: 'Grid',
    components: {
    },
    data: () => ({
      switch1: true,
      mapImage: '',
      hexRadius: 60,
      imageScale: 100,
      imageRotation: 0,
      headerHidden: false,
      windowWidth: window.innerWidth,
      windowHeight: window.innerHeight,

    }),
    computed: {
      hexFontSize() {
        return Math.max(this.hexRadius * 0.3, 6);
      },
      hexes() {
        const r = this.hexRadius;
        const sqrt3 = Math.sqrt(3);
        const hexes = [];
        const width = this.windowWidth;
        const height = this.windowHeight;
        let col = 0;
        let cx = 0;
        while (cx - r <= width) {
          const yOffset = col % 2 === 1 ? (sqrt3 / 2) * r : 0;
          const columnHexes = [];
          let cy = yOffset;
          while (cy - (sqrt3 / 2) * r <= height) {
            const d = [
              `M ${cx + r} ${cy}`,
              `L ${cx + r / 2} ${cy - (sqrt3 / 2) * r}`,
              `L ${cx - r / 2} ${cy - (sqrt3 / 2) * r}`,
              `L ${cx - r} ${cy}`,
              `L ${cx - r / 2} ${cy + (sqrt3 / 2) * r}`,
              `L ${cx + r / 2} ${cy + (sqrt3 / 2) * r}`,
              'Z',
            ].join(' ');
            columnHexes.push({
              id: col + '-' + columnHexes.length,
              col: col,
              row: columnHexes.length,
              cx: cx,
              cy: cy,
              d: d,
            });
            cy += sqrt3 * r;
          }
          // label rows like a chessboard: bottom row of the column = rank 1
          const totalRows = columnHexes.length;
          columnHexes.forEach((hex, index) => {
            hex.label = this.columnLetter(hex.col) + (totalRows - index);
          });
          hexes.push.apply(hexes, columnHexes);
          col += 1;
          cx = col * (3 / 2) * r;
        }
        return hexes;
      },
    },
    mounted() {
      window.addEventListener('resize', this.onResize);
    },
    beforeDestroy() {
      window.removeEventListener('resize', this.onResize);
    },
    methods: {
      columnLetter(colIndex) {
        let n = colIndex + 1;
        let letter = '';
        while (n > 0) {
          const m = (n - 1) % 26;
          letter = String.fromCharCode(65 + m) + letter;
          n = Math.floor((n - 1) / 26);
        }
        return letter;
      },
      onResize() {
        this.windowWidth = window.innerWidth;
        this.windowHeight = window.innerHeight;
      },
    },
  }
</script>
