<template>
  <svg width="7" height="9" viewBox="0 0 7 9" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path d="M3.5 8.5L7 4.5L4.76995e-08 4.5L3.5 8.5Z" :fill="color" />
    <path d="M3.5 0.5L3.5 4.5" :stroke="color" stroke-width="2" />
  </svg>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: "#006CFE"
    },
  },
}
</script>