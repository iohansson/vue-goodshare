<template>
  <span class="counter-facebook"
    v-if="parseFloat(count) > 0"
  >{{ count }}</span>
</template>
<script>
export default {
  name: 'Counter',
  props: {
    count: {
      type: [String, Number],
      default: 0,
    },
  },
}
</script>
