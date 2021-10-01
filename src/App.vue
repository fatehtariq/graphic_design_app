<template>
  <div>
    <top-bar></top-bar>
    <b-container fluid class="pt-2">
      <b-row>
        <b-col cols="3">
          <b-row>
            <side-tab></side-tab>
          </b-row>
          <b-row class="pt-2">
            <canvas-controls></canvas-controls>
          </b-row>
        </b-col>
        <b-col cols="9">
          <b-row>
            <the-canvas></the-canvas> </b-row
        ></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import TopBar from "@/components/TopBar.vue";
import SideTab from "@/components/SideTab.vue";
import TheCanvas from "@/components/TheCanvas.vue";
import CanvasControls from '@/components/controls/CanvasControls.vue';
export default {
  name: "App",
  components: {
    TopBar,
    SideTab,
    TheCanvas,
    CanvasControls
  },
};
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Roboto+Condensed');
</style>