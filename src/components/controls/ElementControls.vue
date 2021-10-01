<template>
  <div>
    <strong>Element Controls</strong>
    <b-container>
      <b-row class="mt-2">
        <label>Border Width</label>
        <b-col>
          <b-form-input
            type="range"
            class="mt-2"
            v-model="borderSize"
            :min="1"
            :max="20"
            @input="changeActiveObjectSize"
          ></b-form-input>
        </b-col>
        <b-col>
          {{ this.borderSize }}
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <label>Fill Color</label>
          <b-form-input
            class="mt-1"
            type="color"
            @input="changeActiveObjectColor"
            v-model="selectedColor"
          ></b-form-input>
        </b-col>
        <b-col>
          <label>Border Color</label>
          <b-form-input
            class="mt-1"
            type="color"
            @input="changeActiveObjectBorderColor"
            v-model="selectedBorderColor"
          ></b-form-input>
        </b-col>
      </b-row>
      <b-row class="mt-2">
        <label>Opacity</label>
        <b-col>
          <b-form-input
            type="range"
            class="mt-2"
            v-model="selectedOpacity"
            :min="0"
            :max="1"
            step="0.01"
            @input="changeActiveObjectOpacity"
          ></b-form-input>
        </b-col>
        <b-col>
          {{ this.selectedOpacity }}
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  props: ["activeObject"],
  data() {
    return {
      task: "",
      borderSize: 0,
      selectedColor: "#000000",
      selectedBorderColor: "#000000",
      selectedOpacity: 1,
    };
  },
  methods: {
    changeActiveObjectOpacity() {
      this.task = "changeSelectedProperty";
      this.$emit("newCanvasTask", {
        task: this.task,
        newProperty: {
          opacity: this.selectedOpacity,
        },
      });
    },
    changeActiveObjectSize() {
      this.task = "changeSelectedProperty";
      this.$emit("newCanvasTask", {
        task: this.task,
        newProperty: {
          strokeWidth: parseInt(this.borderSize),
        },
      });
    },
    changeActiveObjectColor() {
      this.task = "changeSelectedProperty";
      this.$emit("newCanvasTask", {
        task: this.task,
        newProperty: {
          fill: this.selectedColor,
        },
      });
    },
    changeActiveObjectBorderColor() {
      this.task = "changeSelectedProperty";
      this.$emit("newCanvasTask", {
        task: this.task,
        newProperty: {
          stroke: this.selectedBorderColor,
        },
      });
    },
  },
  created() {
    if (this.activeObject != null) {
      this.activeObj = this.activeObject;
      this.borderSize = this.activeObj.strokeWidth;
      this.selectedColor = this.activeObject.fill;
      this.selectedBorderColor = this.activeObject.stroke;
      this.selectedOpacity = this.activeObject.opacity;
    }
  },
};
</script>

<style></style>
