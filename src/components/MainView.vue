<template>
  <div class="container">
    <div></div>
    Has Geo : {{hasGeo}}
    <h1>{{ msg }}</h1>
    <div v-if="position">
      <div>accuracy: {{coords.accuracy || "null"}}</div>
      <div>altitude: {{coords.altitude || "null"}}</div>
      <div>altitudeAccuracy: {{coords.altitudeAccuracy || "null"}}</div>
      <div>heading: {{coords.heading || "null"}}</div>
      <div>latitude: {{coords.latitude || "null"}}</div>
      <div>longitude: {{coords.longitude || "null"}}</div>
      <div>speed: {{coords.speed || "null"}}</div>
      <div>coords: {{coords.latitude + "," + coords.longitude}}</div>
      <div>timestamp: {{position.timestamp}}</div>
      <div>Time: {{(new Date(position.timestamp)).toLocaleString()}}</div>
    </div>
    <div>
      <pre>{{history.map(h=>posToStr(h))}}</pre>
    </div>
  </div>
</template>

<script lang="ts">
type Coords = {
  accuracy: number;
  altitude: number | null;
  altitudeAccuracy: number | null;
  heading: number | null;
  latitude: number;
  longitude: number;
  speed: number | null;
};
type Position = { coords: Coords; timestamp: number };

import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class MainView extends Vue {
  @Prop() private msg!: string;
  hasGeo: boolean;
  position: Position = null;
  history: Position[] = [];
  coords: Coords;
  created() {
    if (navigator.geolocation) {
      this.hasGeo = true;
      console.log("beginning watch");
      navigator.geolocation.watchPosition(this.newPosition);
    } else {
      this.hasGeo = false;
    }
  }
  // get coords() {
  //   return this.position.coords.accuracy || "fart";
  // }

  newPosition(position: Position) {
    this.position = position;
    this.coords = position.coords;
    this.history.push(position);
    console.log("new position", position);
  }
  posToStr(pos: Position): string {
    const coords = pos.coords;
    return `accuracy: ${coords.accuracy} altitude: ${
      coords.altitude
    } altitudeAccuracy: ${coords.altitudeAccuracy} heading: ${
      coords.heading
    } latitude: ${coords.latitude} longitude: ${coords.longitude} speed: ${
      coords.speed
    } Time: ${new Date(pos.timestamp).toLocaleString()}`;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
