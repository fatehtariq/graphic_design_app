<template>
  <div class="px-2 pb-2">
    <b-list-group>
      <b-list-group-item button @click="drawDefaultCircle"
        ><b-icon icon="circle"></b-icon> Circle</b-list-group-item
      >
      <b-list-group-item button @click="drawDefaultTriangle"
        ><b-icon icon="triangle"></b-icon> Triangle</b-list-group-item
      >
      <b-list-group-item button @click="drawDefaultRectangle"
        ><b-icon icon="square"></b-icon> Rectangle</b-list-group-item
      >
    </b-list-group>
  </div>
</template>

<script>
export default {
  data() {
    return {
      task: "none",
    };
  },
  methods: {
    drawDefaultRectangle() {
      this.task = "drawRectangle";
      this.$emit("newCanvasTask", {
        task: this.task,
      });
    },
    drawDefaultCircle() {
      this.task = "drawCircle";
      this.$emit("newCanvasTask", {
        task: this.task,
      });
    },
    drawDefaultTriangle() {
      this.task = "drawTriangle";
      this.$emit("newCanvasTask", {
        task: this.task,
      });
    },
  },
};
</script>
