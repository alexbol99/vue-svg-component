<template>
  <div>
    <svg
        ref="svgContainer"
        @click="handleClick"
        @mousemove="handleMouseMove"
        @keydown="handleKeyDown"
        tabindex="0"
        style="border: 1px solid #ccc"
        width="400"
        height="300"
    >
      <polyline :points="points"
                  fill="none"
                  stroke="black"
                  stroke-width="2"
                />
    </svg>
  </div>
</template>

<script>
export default {
  name: "drawPolyline",
  data() {
    return {
      drawing: true,
      lines: [],
      currentLine: { points: [] },
      lineStarted: false,
    };
  },
  methods: {
    handleClick(event) {
      if (this.drawing) {
        const svgContainer = this.$refs.svgContainer;
        const point = this.getSVGPoint(event, svgContainer);
        if (this.currentLine.points.length === 0) {
          this.currentLine.points.push(point);
        }
        this.lineStarted = false;
        console.log(this.currentLine.points.map(p => `${p.x} ${p.y}`))
      }
    },
    handleMouseMove(ev) {
      if (this.drawing) {
        ev.preventDefault()
        const svgContainer = this.$refs.svgContainer;
        const currentPoint = this.getSVGPoint(ev, svgContainer)
        if (this.lineStarted) {
          this.currentLine.points[this.currentLine.points.length - 1] = currentPoint
        }
        else {
          this.currentLine.points.push(currentPoint)
          this.lineStarted = true
        }
      }
    },
    handleKeyDown(event) {
      if (event.key === "Escape") {
        this.stopDrawing();
      }
    },
    getSVGPoint(event, svgContainer) {
      const point = svgContainer.createSVGPoint();
      point.x = event.clientX;
      point.y = event.clientY;

      return point.matrixTransform(svgContainer.getScreenCTM().inverse());
    },
    stopDrawing() {
      if (this.drawing) {
        this.drawing = false;
        this.lines.push(this.currentLine);
      }
    },
  },
  computed: {
    points() {
      return this.currentLine.points.map(p => `${p.x}, ${p.y}`).join(' ')
    }
  },
  mounted() {
    this.$refs.svgContainer.focus();
  },
};
</script>

<style scoped>
/* Add your component styles here */
</style>
