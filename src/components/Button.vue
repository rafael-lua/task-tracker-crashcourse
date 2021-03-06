<template>
  <button @click="$emit('btn-click')" :style="{ background: color }" class="btn">{{text}}</button> <!-- @click is a directive to v-on:click -->
</template>

<script>
export default {
  name: "Button",
  props: {
    text: String,
    color: String
  },
  emits: [ "btn-click" ]
}
</script>