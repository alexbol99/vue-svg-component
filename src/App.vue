<script>
import SvgComponent from "@/components/svgComponent.vue";
import DrawPolyline from "@/components/drawPolyline.vue";
export default {
  data() {
    return {
      currentComponent: 'DrawPolyline'
    }
  },
  components: {
    SvgComponent,
    DrawPolyline
  }
}
</script>

<template>
  <button @click="currentComponent = 'SvgComponent'">
    Show SvgComponent
  </button>
  <button @click="currentComponent = 'DrawPolyline'">
    Show DrawPolyline
  </button>
<!--  <svg-component></svg-component>-->
<!--  <draw-polyline></draw-polyline>-->
  <component :is="currentComponent"></component>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
