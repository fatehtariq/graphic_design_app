<template>
  <div class="px-2 pb-2">
    <b-list-group>
      <b-list-group-item button @click="drawDefaultText"><b-icon icon="card-text"></b-icon> Add Text</b-list-group-item>
      <b-list-group-item button @click="drawDefaultTextbox"><b-icon icon="textarea-t"></b-icon> Add Textbox</b-list-group-item>
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
    drawDefaultText() {
      this.task = "drawText";
      this.$emit("newCanvasTask", {
        task: this.task,
      });
    },
    drawDefaultTextbox() {
      this.task = "drawTextbox";
      this.$emit("newCanvasTask", {
        task: this.task,
      });
    },
  },
};
</script>
