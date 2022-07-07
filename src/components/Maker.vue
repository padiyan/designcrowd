<template>
  <div class="hello">
    <h1 class="text-center">Maker</h1>
    <div class="">
      <canvas class="" ref="can" width="800" height="800"></canvas>
      <div id=controls>
        <button id="addNewSquare" @click="addNewSquare">Add New Square</button>
        <button id="evenlySpaceVertically" @click="evenlySpaceVertically">Evenly Space Vertically</button>
        <button id="getCurrentObjects" @click="getCurrentCanvasObjects">Console Log Current Objects</button>
        <button id="reRenderCanvas" @click="reRenderCanvas">Re-Render Canvas</button>
      </div>
    </div>
  </div>
</template>

<script>
import { fabric } from 'fabric';

export default {
  name: 'Maker',
  data: () => ({
    canvas: null,
  }),
  mounted() {
    const ref = this.$refs.can;
    this.canvas = new fabric.Canvas(ref);
    const rect = new fabric.Rect({
      fill: 'red',
      width: 100,
      height: 100,
      left:0,
      top:0
    });
    this.canvas.add(rect);
  },
  methods: {
    addNewSquare() {
      //To be implemented
    },
    evenlySpaceVertically(){
      //To be implemented
    },
    reRenderCanvas() {
      this.canvas.renderAll();
    },
    getCurrentCanvasObjects(){
      console.log(this.canvas.getObjects());
      return this.canvas.getObjects();
    },
  }
}
</script>

<style scoped>

</style>
