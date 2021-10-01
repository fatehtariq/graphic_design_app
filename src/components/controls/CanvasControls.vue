<template>
  <div>
    <b-card>
      <b-container>
        <b-row>
          <b-col>
            <text-controls
              v-if="setTextTab"
              :activeObject="this.activeObject"
              @newCanvasTask="changeActiveProperty($event)"
            ></text-controls>
            <element-controls
              v-if="setElementTab"
              :activeObject="this.activeObject"
              @newCanvasTask="changeActiveProperty($event)"
            ></element-controls>
            <free-drawing-controls
              v-if="setDrawingTab"
              @newCanvasTask="changeActiveProperty($event)"
              :brushProperty="this.brushProperty"
            ></free-drawing-controls>
            <controls-loader v-if="setLoaderTab"></controls-loader>
          </b-col>
        </b-row>
      </b-container>
    </b-card>
  </div>
</template>

<script>
import ElementControls from "@/components/controls/ElementControls.vue";
import TextControls from "@/components/controls/TextControls.vue";
import FreeDrawingControls from "@/components/controls/FreeDrawingControls.vue";
import ControlsLoader from "@/components/controls/ControlsLoader.vue";

export default {
  computed: {
    setTextTab() {
      if (this.activeTab === "textTab") {
        return true;
      }
      return false;
    },
    setElementTab() {
      if (this.activeTab === "elementTab") {
        return true;
      }
      return false;
    },
    setDrawingTab() {
      if (this.activeTab === "freeDrawingTab") {
        return true;
      }
      return false;
    },
    setLoaderTab() {
      if (this.activeTab === "loaderTab") {
        return true;
      }
      return false;
    },
  },
  components: {
    ElementControls,
    TextControls,
    FreeDrawingControls,
    ControlsLoader,
  },
  data() {
    return {
      task: "none",
      activeObject: null,
      activeTab: "textTab",
      brushProperty: null,
    };
  },
  methods: {
    changeActiveProperty(propertChange) {
      this.$root.$emit("newCanvasTask", {
        task: propertChange.task,
        newProperty: propertChange.newProperty,
      });
    },
  },
  beforeMount() {
    this.$root.$on("changeTab", (newTab) => {
      this.activeTab = newTab;
    });
  },
  mounted() {
    this.$root.$on("setActiveProperties", (activeObj) => {
      this.activeObject = activeObj.activeObject;
    });

    this.$root.$on("setBrushProperties", (brushProperty) => {
      this.brushProperty = brushProperty;
    });
  },
};
</script>
