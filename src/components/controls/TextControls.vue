<template>
  <div>
    <strong>Text Controls</strong>
    <b-container>
      <b-row class="mt-2">
        <b-col>
          <label>Color</label>
          <b-form-input
            class="mt-1"
            type="color"
            @input="changeActiveObjectColor"
            v-model="selectedColor"
          ></b-form-input>
        </b-col>
        <b-col>
          <label>Font Family</label>
          <b-form-select
            class="mt-2"
            v-model="fontSelected"
            :options="fontOptions"
            @input="changeActiveObjectFont"
          ></b-form-select>
        </b-col>
      </b-row>
      <b-row class="mt-2">
        <label>Font Size</label>
        <b-col>
          <b-form-input
            type="range"
            class="mt-2"
            v-model="sizeSelected"
            :min="10"
            :max="150"
            @input="changeActiveObjectSize"
          ></b-form-input>
        </b-col>
        <b-col>
          {{ this.sizeSelected }}
        </b-col>
      </b-row>
      <b-row class="mt-2">
        <label>Font Weight</label>
        <b-button-group>
          <b-button variant="light" @click="changeWeight('bold')"
            ><strong>Bold</strong></b-button
          >
          <b-button variant="light" @click="changeWeight('normal')"
            >Normal</b-button
          >
        </b-button-group>
      </b-row>
      <b-row class="mt-2">
        <label>Font Style</label>
        <b-button-group>
          <b-button variant="light" @click="changeActiveObjectStyle('italic')"
            ><i>Italic</i></b-button
          >
          <b-button
            variant="light"
            @click="changeActiveObjectStyle('underline')"
            ><u>Underline</u></b-button
          >
        </b-button-group>
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
import { fabric } from "fabric";
export default {
  props: ["activeObject"],
  computed: {
    isDisabled() {
      if (this.activeObject instanceof fabric.Text) {
        return false;
      }
      return true;
    },
  },
  data() {
    return {
      controlDisabled: true,
      task: "",
      isUnderline: false,
      activeObj: null,
      selectedStyle: "normal",
      selectedColor: "#000000",
      sizeSelected: 20,
      weightSelected: "normal",
      fontSelected: "times new Roman",
      fontOptions: [
        { value: "arial", text: "Arial" },
        { value: "times new roman", text: "Times New Roman" },
      ],
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
    changeWeight(weight) {
      this.weightSelected = weight;
      this.changeActiveObjectWeight();
    },
    changeActiveObjectFont() {
      this.task = "changeSelectedProperty";
      this.$emit("newCanvasTask", {
        task: this.task,
        newProperty: {
          fontFamily: this.fontSelected,
        },
      });
    },
    changeActiveObjectSize() {
      this.task = "changeSelectedProperty";
      this.$emit("newCanvasTask", {
        task: this.task,
        newProperty: {
          fontSize: this.sizeSelected,
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
    changeActiveObjectWeight() {
      this.task = "changeSelectedProperty";
      this.$emit("newCanvasTask", {
        task: this.task,
        newProperty: {
          fontWeight: this.weightSelected,
        },
      });
    },
    changeActiveObjectStyle(type) {
      this.task = "changeSelectedProperty";
      if (type === "italic") {
        if (this.selectedStyle === "italic") {
          this.$emit("newCanvasTask", {
            task: this.task,
            newProperty: {
              fontStyle: "normal",
            },
          });
          this.selectedStyle = "normal";
        } else if (this.selectedStyle === "normal") {
          this.$emit("newCanvasTask", {
            task: this.task,
            newProperty: {
              fontStyle: "italic",
            },
          });
          this.selectedStyle = "italic";
        }
      } else if (type === "underline") {
        if (this.isUnderline === true) {
          this.$emit("newCanvasTask", {
            task: this.task,
            newProperty: {
              underline: false,
            },
          });
          this.isUnderline = false;
        } else {
          this.isUnderline = true;
          this.$emit("newCanvasTask", {
            task: this.task,
            newProperty: {
              underline: true,
            },
          });
        }
      }
    },
  },
  created() {
    if (this.activeObject != null) {
      this.activeObj = this.activeObject;
      this.selectedColor = this.activeObj.fill;
      this.sizeSelected = this.activeObject.fontSize;
      this.fontSelected = this.activeObject.fontFamily;
      this.weightSelected = this.activeObject.fontWeight;
      this.selectedStyle = this.activeObject.fontStyle;
      this.selectedOpacity = this.activeObject.opacity;
      this.isUnderline = this.activeObject.underline;
    }
  },
};
</script>

<style></style>
