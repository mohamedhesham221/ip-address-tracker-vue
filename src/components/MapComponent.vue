<template>
<div style="height: 100%; width: 100%; position: absolute; top: 240px; z-index: 0;">
  <div class="map">
    <l-map
      v-if="showMap"
      :zoom="zoom"
      :center="center"
      :options="mapOptions"
      style="height: 100%"
      @update:center="centerUpdate"
      @update:zoom="zoomUpdate"
    >
      <l-tile-layer
        :url="url"
        :attribution="attribution"
      />
      <l-marker :lat-lng="withPopup">
        <l-popup>
          <div>
            {{ city }}
          </div>
        </l-popup>
      </l-marker>
    </l-map>
  </div>
  </div>
</template>
<script>
import {
  LMap, LTileLayer, LMarker, LPopup,
} from 'vue2-leaflet';
import { latLng } from 'leaflet';
import EventBus from '../main';

export default {
  name: 'MapComponents',
  data() {
    return {
      zoom: 13,
      center: latLng(47.41322, -1.219482),
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      withPopup: latLng(47.41322, 1.219482),
      currentZoom: 11.5,
      currentCenter: latLng(47.41322, -1.219482),
      showParagraph: false,
      mapOptions: {
        zoomSnap: 0.5,
      },
      showMap: true,
      myRes: null,
      city: null,
    };
  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
  },
  created() {
    this.emitData();
  },
  methods: {
    zoomUpdate(zoom) {
      this.currentZoom = zoom;
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    emitData() {
    // eslint-disable-next-line no-return-assign
      EventBus.$on('data', (val) => {
        this.myRes = val;
        this.city = this.myRes.location.city;
        this.center = latLng(this.myRes.location.lat, this.myRes.location.lng);
        this.withPopup = latLng(this.myRes.location.lat, this.myRes.location.lng);
        this.currentCenter = latLng(this.myRes.location.lat, this.myRes.location.lng);
      });
    },
  },
};
</script>
<style scoped>
.map {
  width: 100%;
  height: 100vh;
  overflow: auto;
}
</style>
