<template>
  <div>
    <b-card>
      <b-container class="pt-2">
        <b-row>
          <b-col class="mt-1">
            <input type="file" ref="file" @input="drawImage($event)" />
          </b-col>
          <b-col cols="8">
            <base-controls
              class="pb-2"
              @newCanvasTask="taskListener"
            ></base-controls>
          </b-col>
        </b-row>
      </b-container>
    </b-card>
    <b-card class="mt-2">
      <canvas ref="myCanvas" width="1100" height="570"></canvas>
    </b-card>
  </div>
</template>

<script>
import { fabric } from "fabric";
import "fabric-history";
import BaseControls from "@/components/controls/BaseControls.vue";

export default {
  components: {
    BaseControls,
  },
  data() {
    return {
      task: "",
      canvas: null,
    };
  },
  methods: {
    drawTriangle(canvas) {
      const triangle = new fabric.Triangle({
        width: 150,
        height: 150,
        strokeWidth: 1,
        stroke: "black",
      });
      canvas.add(triangle);
      canvas.centerObject(triangle);
    },
    drawCircle(canvas) {
      const circle = new fabric.Circle({
        radius: 50,
        fill: "black",
        strokeWidth: 1,
        stroke: "black",
      });
      canvas.add(circle);
      canvas.centerObject(circle);
    },
    drawRectangle(canvas) {
      const rect = new fabric.Rect({
        fill: "black",
        width: 150,
        height: 250,
        scalingAffectsSizeProp: true,
        strokeWidth: 5,
        stroke: "black",
      });
      canvas.add(rect);
      canvas.centerObject(rect);
    },
    drawImage(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = (f) => {
        const data = f.target.result;
        fabric.Image.fromURL(data, (img) => {
          var oImg = img
            .set({ left: 0, top: 0, angle: 0, width: 1000, height: 800 })
            .scale(0.9);
          this.canvas.add(oImg).renderAll();
          this.canvas.setActiveObject(oImg);
          this.canvas.centerObject(oImg);
          this.canvas.toDataURL({ format: "png", quality: 0.8 });
        });
      };
      reader.readAsDataURL(file);
    },
    drawText(canvas) {
      const text = new fabric.Text("This is some Default Text", {
        fontFamily: "times new roman",
      });
      canvas.add(text);
      canvas.centerObject(text);
    },
    drawTextbox(canvas) {
      const textbox = new fabric.Textbox(
        "This is some Default Textbox, click to change",
        {
          fontFamily: "times new roman",
        }
      );
      canvas.add(textbox);
      canvas.centerObject(textbox);
    },
    clearCanvas(canvas) {
      canvas.clear();
    },
    redoChange(canvas) {
      canvas.redo();
    },
    undoChange(canvas) {
      canvas.undo();
    },
    changeActiveSelectedProperty(obj) {
      const activeObj = this.canvas.getActiveObject();
      if (activeObj === null) {
        console.log("No object selected");
        return;
      }
      console.log("Active selected: " + activeObj);
      activeObj.set(obj);
      this.canvas.requestRenderAll();
    },
    drawingMode(canvas) {
      canvas.isDrawingMode = !canvas.isDrawingMode;
    },
    changeBrushProperty(canvas, property) {
      const brush = canvas.freeDrawingBrush;
      brush.width = parseInt(property.width, 10) || 1;
      brush.color = property.color;

      this.$root.$emit("setBrushProperties", {
        width: brush.width,
        color: brush.color,
      });
    },
    sendForward(canvas) {
      const activeObj = canvas.getActiveObject();
      canvas.bringForward(activeObj);
      canvas.requestRenderAll();
    },
    sendBackward(canvas) {
      const activeObj = canvas.getActiveObject();
      canvas.sendBackwards(activeObj);
      canvas.requestRenderAll();
    },
    taskListener(canvas) {
      this.$root.$on("newCanvasTask", (newTask) => {
        this.task = newTask;
        console.log(this.task.task + " task received at Canvas Component");
        if (this.task.task === "drawRectangle") {
          this.drawRectangle(canvas);
        } else if (this.task.task === "drawCircle") {
          this.drawCircle(canvas);
        } else if (this.task.task === "drawTriangle") {
          this.drawTriangle(canvas);
        } else if (this.task.task === "drawText") {
          this.drawText(canvas);
        } else if (this.task.task === "drawTextbox") {
          this.drawTextbox(canvas);
        } else if (this.task.task === "clearCanvas") {
          this.clearCanvas(canvas);
        } else if (this.task.task === "redoChange") {
          this.redoChange(canvas);
        } else if (this.task.task === "undoChange") {
          this.undoChange(canvas);
        } else if (this.task.task === "changeSelectedProperty") {
          console.log("New property change: " + this.task.newProperty);
          this.changeActiveSelectedProperty(this.task.newProperty);
        } else if (this.task.task === "enterDrawingMode") {
          this.drawingMode(canvas);
        } else if (this.task.task === "changeBrushProperty") {
          this.changeBrushProperty(canvas, this.task.newProperty);
        } else if (this.task.task === "sendForward") {
          this.sendForward(canvas);
        } else if (this.task.task === "sendBackward") {
          this.sendBackward(canvas);
        } else if (this.task.task === "downloadCanvas") {
          this.downloadCanvas(canvas);
        } else if (this.task.task === "randomizeItems") {
          this.randomizeItems(canvas);
        }
      });
    },
    updateControls() {
      this.canvas.on({
        "selection:updated": () => {
          console.log("New Active Object Selected");
          const activeObj = this.canvas.getActiveObject();
          this.$root.$emit("setActiveProperties", {
            task: "setActiveProperties",
            activeObject: activeObj,
          });
        },
        "selection:created": () => {
          console.log("New Active Object Selected");
          const activeObj = this.canvas.getActiveObject();
          this.$root.$emit("setActiveProperties", {
            task: "setActiveProperties",
            activeObject: activeObj,
          });
        },
        "selection:cleared": () => {
          console.log("Active Object Selection Cleared");
          this.$root.$emit("clearSelection", {
            task: "clearSelection",
            isDisabled: true,
          });
        },
      });
    },
    zoomControls(canvas) {
      canvas.on("mouse:wheel", (opt) => {
        var delta = opt.e.deltaY;
        var zoom = canvas.getZoom();
        zoom *= 0.999 ** delta;
        if (zoom > 20) zoom = 20;
        if (zoom < 0.01) zoom = 0.01;
        canvas.setZoom(zoom);
        opt.e.preventDefault();
        opt.e.stopPropagation();
      });
    },
    downloadCanvas(canvas) {
      const image = canvas
        .toDataURL("image/png", 1.0)
        .replace("image/png", "image/octet-stream");
      const link = document.createElement("a");
      link.download = "image.png";
      link.href = image;
      link.click();
    },
    randomizeItems(canvas) {
      for(let i = 0 ; i < 1000 ; i++) {
       canvas.add( new fabric.Rect({
        top: Math.random() * canvas.height,
        left: Math.random() * canvas.width,
        width: 80,
        height: 50,
        fill: "black",
        }));
      }
    },
  },
  mounted() {
    const ref = this.$refs.myCanvas;
    const canvas = new fabric.Canvas(ref, {
      preserveObjectStacking: true,
      backgroundColor: "#fff",
    });
    this.canvas = canvas;
    this.taskListener(canvas);
    this.updateControls();
    this.zoomControls(canvas);
  },
};
</script>
