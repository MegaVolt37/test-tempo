<template>
  <div class="progressbar">
    {{ percent }}%<div class="line"><span :style="`width: ${percent}%;${styleBackgroundLine}`"></span>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    styleBackgroundLine() {
      if (this.percent < 25) {
        return "background-color: #FF0000";
      } else if (this.percent >= 25 && this.percent < 75) {
        return "background-color: #FFA200";
      } else {
        return "background-color: #01AA88";
      }
    },
  },
  props: {
    percent: { Number },
  }
}
</script>

<style lang="scss" scoped>
.progressbar {
  display: flex;
  align-items: center;
  width: 100%;
  gap: 7px;

  .line {
    height: 4px;
    flex: 1;
    background-color: $blue-light;
    overflow: hidden;
    border-radius: 4px;

    span {
      display: block;
      height: 4px;
    }
  }
}
</style>