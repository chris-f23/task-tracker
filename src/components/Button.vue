<template>
  <button @click="$emit('btn-click')" :class="['btn', `btn-${color}`]">{{texto}}</button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    texto: String,
    color: String
  },
  emits: ['btn-click']
}
</script>