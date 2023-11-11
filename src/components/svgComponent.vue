<template>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600" ref="mainsvg"
         style="border: solid"
    >
      <rect v-for="elm in geom" class ="myRect"
            :id="elm.id"
            :x="elm.position.x" :y="elm.position.y"
            width="20%" height="20%"
            @mousedown="event => startDrag(event, elm)"
      />
    </svg>
</template>

<script>
import {defineComponent} from 'vue'

export default defineComponent({
  name: "svgComponent",
  data() {
    return {
      geom: [
        {
          position: {x: 5, y: 10},
          id: "rect_1"
        },
        {
          position: {x: 205, y: 10},
          id: "rect_2"
        }
      ],
      dragOffset: null,
      dragId: null
    }
  },
  methods: {
    screenToSVG(x, y) {
      const svgRef = this.$refs.mainsvg
      let point = svgRef.createSVGPoint();
      point.x = x;
      point.y = y;
      let cursor = point.matrixTransform(svgRef.getScreenCTM().inverse())
      return cursor
    },
    startDrag(event, elm) {
      event.preventDefault()
      console.log(`start drag elm: ${elm.id}`)
      document.addEventListener("mousemove", this.onMouseMove);
      document.addEventListener("mouseup", this.onMouseUp);
      const anchorX = elm.position.x;
      const anchorY = elm.position.y;
      let cursor = this.screenToSVG(event.clientX, event.clientY)
      this.dragOffset = {
        x: cursor.x - anchorX,
        y: cursor.y - anchorY
      }
      this.dragId = event.target.id
    },
    onMouseMove(event) {
      event.preventDefault()
      let cursor = this.screenToSVG(event.clientX, event.clientY)
      const position = {
        x: cursor.x - this.dragOffset.x,
        y: cursor.y - this.dragOffset.y
      }
      const id = this.dragId
      this.updateElement(id, position)
    },
    onMouseUp() {
      this.cleanDrag()
    },
    cleanDrag() {
      document.removeEventListener("mousemove", this.onMouseMove);
      document.removeEventListener("mouseup", this.onMouseUp);
      this.dragOffset = {x: 0, y: 0}
      this.dragId = null
    },
    updateElement(id, position) {
      this.geom = this.geom.map(elm => {
        return elm.id === id ? {...elm, position: position} : elm
      })
    }

  }
})
</script>

<style scoped>
.myRect {
  fill: greenyellow;
}

.myRect:hover {
  fill: darkorange;
}
</style>
