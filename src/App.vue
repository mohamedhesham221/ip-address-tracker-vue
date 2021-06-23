<template>
  <div id="app">
    <div class="container">
      <ip-tracker />
    </div>
    <map-component />
  </div>
</template>

<script>
import IpTracker from './components/IpTracker.vue';
import MapComponent from './components/MapComponent.vue';

export default {
  name: 'App',
  components: {
    IpTracker,
    MapComponent,
  },
};
</script>

<style>
:root {
    --veryDarkGray: hsl(0, 0%, 17%);
    --darkGray: hsl(0, 0%, 59%);
    --white: #fff;
    font-size: 18px;
}
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Rubik', sans-serif;
}
.container {
    width: 100%;
    height: max-content;
    text-align: center;
    position: absolute;
    top: 0;
    z-index: 1;
}

</style>
