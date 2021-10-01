<template>
  <div>
    <b-button variant="warning" @click="randomizeItems">
      <b-icon icon="exclamation-triangle-fill" aria-hidden="true"></b-icon
      >Randomize Items
    </b-button>
    <b-button variant="light" @click="sendForward" class="mx-2"
      >Send Forward
    </b-button>
    <b-button variant="light" @click="sendBackward">Send Backward</b-button>

    <b-button-group class="mx-2">
      <b-button variant="light" @click="undoChange"
        ><b-icon icon="arrow90deg-left" aria-hidden="true"></b-icon
      ></b-button>
      <b-button variant="light" @click="redoChange"
        ><b-icon icon="arrow90deg-right" aria-hidden="true"></b-icon
      ></b-button>
    </b-button-group>

    <b-button variant="success" @click="downloadCanvas" class="mr-2"
      ><b-icon icon="arrow-down" aria-hidden="true"></b-icon
    ></b-button>

    <b-button class="mx-2" variant="danger" @click="clearCanvas"
      ><b-icon icon="trash" aria-hidden="true"></b-icon
    ></b-button>
  </div>
</template>

<script>
export default {
  computed: {
    elementControlsActive() {
      return false;
    },
    textControlsActive() {
      return true;
    },
  },
  components: {},
  data() {
    return {
      task: "none",
    };
  },
  methods: {
    randomizeItems() {
      this.task = "randomizeItems";
      this.$root.$emit("newCanvasTask", {
        task: this.task,
      });
      console.log("Canvas randomize Items task emitted");
    },
    downloadCanvas() {
      this.task = "downloadCanvas";
      this.$root.$emit("newCanvasTask", {
        task: this.task,
      });
      console.log("Canvas download task emitted");
    },
    sendForward() {
      this.task = "sendForward";
      this.$root.$emit("newCanvasTask", {
        task: this.task,
      });
      console.log("Canvas item forward task emitted");
    },
    sendBackward() {
      this.task = "sendBackward";
      this.$root.$emit("newCanvasTask", {
        task: this.task,
      });
      console.log("Canvas item backward task emitted");
    },
    clearCanvas() {
      this.task = "clearCanvas";
      this.$root.$emit("newCanvasTask", {
        task: this.task,
      });
      console.log("Canvas clear task emitted");
    },
    undoChange() {
      this.task = "undoChange";
      this.$root.$emit("newCanvasTask", {
        task: this.task,
      });
      console.log("Canvas undo task emitted");
    },
    redoChange() {
      this.task = "redoChange";
      this.$root.$emit("newCanvasTask", {
        task: this.task,
      });
      console.log("Redo change task emitted");
    },
  },
};
</script>
