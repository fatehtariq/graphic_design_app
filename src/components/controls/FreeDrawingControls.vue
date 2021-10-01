<template>
  <div>
    <strong>Drawing Controls</strong>
    <b-container>
      <b-row>
        <b-col>
          <label>Color</label>
          <b-form-input
            class="mt-1"
            type="color"
            @input="changeBrushProperty"
            v-model="selectedColor"
          ></b-form-input>
        </b-col>
      </b-row>
      <b-row>
        <label>Line Width</label>
        <b-col>
          <b-form-input
            type="range"
            class="mt-2"
            v-model="selectedSize"
            :min="1"
            :max="50"
            @input="changeBrushProperty"
          ></b-form-input>
        </b-col>
        <b-col>
          {{ this.selectedSize }}
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  props: ["brushProperty"],
  data() {
    return {
      task: "",
      selectedSize: 10,
      selectedColor: "#000000",
      brushProp: null,
    };
  },
  methods: {
    changeBrushProperty() {
      this.task = "changeBrushProperty";
      this.$emit("newCanvasTask", {
        task: this.task,
        newProperty: {
          width: this.selectedSize,
          color: this.selectedColor,
        },
      });
    },
  },
  created() {
    if (this.brushProperty != null) {
      this.brushProp = this.brushProperty;
      this.selectedSize = this.brushProp.width;
      this.selectedColor = this.brushProp.color;
    }
  },
};
</script>
