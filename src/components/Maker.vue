<template>
  <div class="p-8">
    <Header :text="headerText"/>

    <div class="flex">
      <div class="p-2 border-2 border-dashed border-indigo-200">
        <canvas class="" ref="can" width="1300" height="800"></canvas>
      </div>

      <div class="flex flex-col pl-8">
        <div id="controls" class="flex flex-col gap-8 p-8 border-2 border-dashed border-indigo-200">
          <p class="text-2xl font-semibold">Control Panel</p>
          <Button id="addNewSquare" name="Add New Square" :clickHandler="addNewSquare" />
          <Button id="evenlySpaceVertically" name="Evenly Space Vertically" :clickHandler="evenlySpaceVertically" />
          <Button id="getCurrentObjects" name="Console Log Current Objects" :clickHandler="getCurrentCanvasObjects" />
          <Button id="reRenderCanvas" name="Re-Render Canvas" :clickHandler="reRenderCanvas" />
      </div>

      <div id="edit" class="flex flex-col gap-8 mt-20 p-8 border-2 border-dashed border-indigo-200">
        <p class="text-2xl font-semibold">Update Panel</p>

        <div>
          <label for="color" class="block mb-2 text-sm text-left font-medium text-gray-900 dark:text-white">Color Picker</label>
          <select name="color" id="color" ref="color" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-100 focus:border-blue-100 block w-full p-2 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-100 dark:focus:border-blue-100">
            <option v-for="option in options" :value="option.value" :key="option.name">
              {{ option.name }}
            </option>
          </select>
        </div>

        <Button id="changeColor" name="Change Color" :clickHandler="setColor" />

        <div class="flex flex-row gap-28">
          <Button id="undo" name="Undo" :clickHandler="undo" :disabled="disableUndo" />
          <Button id="redo" name="Redo" :clickHandler="redo" :disabled="disableRedo" />
        </div>

      </div>

      </div>

    </div>

  </div>
</template>

<script>
import { fabric } from 'fabric';
import Button from './Button.vue';
import Header from './Header.vue';

export default {
  name: 'Maker',
  components: {
    Button,
    Header,
  },

  data: () => ({
    headerText: "Logo Maker",
    canvas: null,
    colorRef: null,
    newleft: 0,
    newtop: 0,
    history: [],
    historyPosition: 0,
    disbaledClass: 'cursor-not-allowed opacity-50',
    enabledClass: 'hover:bg-blue-700',
    disableRedoButton: true,
    options: [
      {
        name: 'Please select a colour',
        value: '',
      },
      {
        name: 'Red',
        value: 'red',
      },
      {
        name: 'Blue',
        value: 'blue',
      },
      {
        name: 'Green',
        value: 'green',
      },
      {
        name: 'Yellow',
        value: 'yellow',
      },
      {
        name: 'Orchid',
        value: 'orchid',
      },
      {
        name: 'Indigo',
        value: 'indigo',
      },
      {
        name: 'Teal',
        value: 'teal',
      },
      {
        name: 'Silver',
        value: 'silver',
      },
    ]
  }),

  mounted() {
    const canvasRef = this.$refs.can;
    this.colorRef = this.$refs.color;
    this.canvas = new fabric.Canvas(canvasRef);
  },

  computed: {
    disableUndo() {
      return this.historyPosition <= 0
    },
    disableRedo() {
      return this.disableRedoButton || this.historyPosition === this.history.length
    },
  },

  methods: {
    addNewSquare() {
      const rect = new fabric.Rect({
      fill: 'red',
      width: 100,
      height: 100,
      left:this.newleft,
      top:this.newtop,
    });

    this.canvas.add(rect);
    this.newleft += 110
    this.newtop += 110

    },

    evenlySpaceVertically(){
      this.canvas.forEachObject((item) => {
        this.canvas.fxCenterObjectH(item)
      })
    },

    reRenderCanvas() {
      this.canvas.renderAll();
    },

    getCurrentCanvasObjects(){
      console.log(this.canvas.getObjects());
      return this.canvas.getObjects();
    },

    addHistory() {
      let color = this.colorRef.value
      let state = {}
      this.canvas.getActiveObjects().forEach((item) => {
        let itemIndex = this.canvas.getObjects().indexOf(item)
        state[itemIndex] = {
          prevColor: item.get('fill'),
          color,
        }
      })
      this.history = this.history.slice(0, this.historyPosition)
      let historyObj = this.tracker(state)
      this.history.push(historyObj)
      this.historyPosition += 1
      historyObj.execute()
    },

    tracker(state) {
      const operation = {
        undo: 'UNDO',
        execute: 'EXECUTE',
      }
      return {
        state: {...state},
        canvas: this.canvas,
        execute() {
          this.perform('EXECUTE')
        },
        undo() {
          this.perform('UNDO')
        },
        perform(type) {
          for (const [key, value] of Object.entries(this.state)) {
            let item = this.canvas.item(key)
            if (type === operation.undo) {
              item.set('fill', value.prevColor)
            }
            if (type === operation.execute) {
              item.set('fill', value.color)
            }
            this.canvas.renderAll();
          }
        }
      }
    },

    setColor() {
      if (this.canvas.getActiveObjects().length <= 0) {
        alert('Please select a Square/Squares to edit')
        return
      }
      if (this.colorRef.value === "") {
        alert('Please select a valid color')
        return
      }
      this.addHistory()
    },

    undo() {
      if (this.historyPosition > 0) {
        this.disableRedoButton = false;
        this.historyPosition -= 1
        let historyObj = this.history[this.historyPosition]
        historyObj.undo();
      }
    },

    redo() {
      if (this.historyPosition < this.history.length) {
        let historyObj = this.history[this.historyPosition]
        historyObj.execute();
        this.historyPosition += 1;
      }
    },

  }
}
</script>

<style scoped>

</style>
