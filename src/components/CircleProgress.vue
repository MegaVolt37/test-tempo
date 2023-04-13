<template>
  <svg class="pie" :style="styleColorSvg" width="40" viewBox="0 0 37 37">
    <circle r="15.9155" cx="50%" cy="50%"></circle>
    <circle r="15.9155" cx="50%" cy="50%" :style="styleCircle"></circle>
    <text x="50%" y="50%">{{ percent }}%</text>
  </svg>
</template>

<script>
export default {
  computed: {
    styleColorSvg() {
      if (this.percent < 25) {
        return "color: #FF0000;";
      } else if (this.percent >= 25 && this.percent < 75) {
        return "color: #FFA200;";
      } else {
        return "color: #01AA88;";
      }
    },
    styleCircle() {
      return `stroke-dasharray: ${this.percent} 100;`
    },
  },
  props: {
    percent: { Number },
  }
}
</script>

<style lang="scss">
.pie {
  color: limegreen;
}

.pie text {
  fill: $black;
  text-anchor: middle;
  dominant-baseline: central;
  @extend %text-400-14;
  font-size: 11px;
}

.pie circle {
  stroke-width: 3;
}

.pie circle:nth-child(1) {
  fill: $white;
  stroke: $blue-light;
}

.pie circle:nth-child(2) {
  fill: none;
  stroke: currentColor;
  stroke-dasharray: 70 100;
  stroke-linecap: round;
  transform: rotate(-90deg);
  transform-origin: center center;
}
</style>