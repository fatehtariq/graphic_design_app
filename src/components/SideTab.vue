<template>
  <div>
    <b-card no-body>
      <b-tabs content-class="mt-3" justified>
        <b-tab @click="setActiveTab('textTab')">
          <template #title>
            <b-icon icon="cursor-text" variant="dark"></b-icon> Text
          </template>
          <text-tab @newCanvasTask="assignNewCanvasTask($event)"></text-tab
        ></b-tab>
        <b-tab @click="setActiveTab('elementTab')">
          <template #title>
            <b-icon icon="triangle" variant="dark"></b-icon> Element
          </template>
          <element-tab
            @newCanvasTask="assignNewCanvasTask($event)"
          ></element-tab
        ></b-tab>
        <b-tab @click="setActiveTab('freeDrawingTab')">
          <template #title>
            <b-icon icon="pencil" variant="dark"></b-icon> Drawing
          </template>
          <free-drawing-tab
            @newCanvasTask="assignNewCanvasTask($event)"
          ></free-drawing-tab
        ></b-tab>
      </b-tabs>
    </b-card>
  </div>
</template>

<script>
import ElementTab from "@/components/tabControls/ElementTab.vue";
import TextTab from "@/components/tabControls/TextTab.vue";
import FreeDrawingTab from "@/components/tabControls/FreeDrawingTab.vue";
import { fabric } from "fabric";

export default {
  data() {
    return {
      activeTab: "textTab",
      drawingModeIsActive: false,
      activeObject: null,
    };
  },
  components: {
    ElementTab,
    TextTab,
    FreeDrawingTab,
  },
  methods: {
    setActiveTab(tab) {
      this.activeTab = tab;
      if (this.drawingModeIsActive === true) {
        this.drawingModeIsActive = false;
        this.activateDrawingMode();
      }
      this.$root.$emit("changeTab", tab);
      if (tab === "freeDrawingTab") {
        this.activateDrawingMode();
        this.drawingModeIsActive = true;
      }
    },
    assignNewCanvasTask(newTask) {
      this.$root.$emit("newCanvasTask", newTask);
    },
    activateDrawingMode() {
      this.$root.$emit("newCanvasTask", { task: "enterDrawingMode" });
    },
  },
  beforeMount() {
    this.$root.$on("setActiveProperties", (activeObj) => {
      console.log("Old Active Obj:" + this.activeObject);
      this.activeObject = activeObj.activeObject;
      console.log("New Active Obj:" + this.activeObject);

      if (this.activeObject instanceof fabric.Text) {
        setTimeout(() => {
          this.setActiveTab("textTab");
        }, 1);
        this.setActiveTab("loaderTab");
      } else if (
        this.activeObject instanceof fabric.Circle ||
        this.activeObject instanceof fabric.Triangle ||
        this.activeObject instanceof fabric.Rect ||
        this.activeObject instanceof fabric.Image
      ) {
        setTimeout(() => {
          this.setActiveTab("elementTab");
        }, 1);
        this.setActiveTab("loaderTab");
      } else if (this.activeObject instanceof fabric.freeDrawingBrush) {
        //Do nothing yet
      }
    });
  },
};
</script>
