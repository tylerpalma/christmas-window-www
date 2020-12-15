<template>
  <div id="app">
    <div v-for="i in 200" :key="i" class="snow"></div>
    <div class="row">
      <h1>Sandy Pointe Lights</h1>
    </div>
    <div 
      class="row"
      v-for="(button) in buttons"
      :key="button.text">
      <button
        class="btn"
        @click="submitChoice(button)">
        {{ button.text }}
      </button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      buttons: [
        {
          text: 'Christmas Circle',
          path: '/christmasCircle',
          isPending: false,
        },
        {
          text: 'Snowflakes',
          path: '/snowflake',
          isPending: false,
        },
        {
          text: 'Yule Log',
          path: '/fire',
          isPending: false,
        },
        {
          text: 'Rainbow Circle',
          path: '/rainbowCircle',
          isPending: false,
        },
        {
          text: 'Rainbow Glitter',
          path: '/rainbowWithGlitter',
          isPending: false,
        },
        {
          text: 'Pulse',
          path: '/bpm',
          isPending: false,
        },
      ]
    }
  },
  methods: {
    async submitChoice(item) {
      item.isPending = true;
      await axios.get(`http://192.168.1.225${item.path}`);
      item.isPending = false;
    },
  },
}
</script>

<style lang="scss">
body {
  height: 100%;
  max-width: 100%;
  overflow-x: hidden;
  padding: 0;
  margin: 0;
  font-family: 'Ubuntu', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  background-color: #0F8A5F;
}

#app {
  height: 100%;
  width: 100%;
  padding: 0;
  margin: 0;
  overflow: hidden;
}

.row {
  max-width: 100%;
  padding: 0 30px;
  margin: 15px 0;
}

.btn {
  width: 100%;
  max-width: 300px;
  padding: 15px 30px;
  border-radius: 10px;
  border: 0;
  box-shadow: 0px 4px 0px #CC231E;
  background-color: #F5624D;
  color: #ffffff;
  cursor: pointer;
  font-family: 'Ubuntu', sans-serif;
  font-size: 1.4rem;
}

.btn:hover {
  transition: all 100ms linear;
  color: #ffffff;
  background-color: #F5624D;
  box-shadow: 0px 0px 0px #CC231E;
  transform: translateY(4px);
}

.btn:focus {
  outline: 0;
}

@function random_range($min, $max) {
  $rand: random();
  $random_range: $min + floor($rand * (($max - $min) + 1));
  @return $random_range;
}

.snow {
  $total: 200;
  position: absolute;
  width: 5px;
  height: 5px;
  background: white;
  border-radius: 50%;

  @for $i from 1 through $total {
    $random-x: random(1000000) * 0.0001vw;
    $random-offset: random_range(-100000, 100000) * 0.0001vw;
    $random-x-end: $random-x + $random-offset;
    $random-x-end-yoyo: $random-x + ($random-offset / 2);
    $random-yoyo-time: random_range(30000, 80000) / 100000;
    $random-yoyo-y: $random-yoyo-time * 100vh;
    $random-scale: random(10000) * 0.0001;
    $fall-duration: random_range(10, 30) * 1s;
    $fall-delay: random(30) * -1s;

    &:nth-child(#{$i}) {
      opacity: random(10000) * 0.0001;
      transform: translate($random-x, -10px) scale($random-scale);
      animation: fall-#{$i} $fall-duration $fall-delay linear infinite;
    }

    @keyframes fall-#{$i} {
      #{percentage($random-yoyo-time)} {
        transform: translate($random-x-end, $random-yoyo-y) scale($random-scale);
      }
      
      to {
        transform: translate($random-x-end-yoyo, 100vh) scale($random-scale);
      }
    }
  }
}

</style>
