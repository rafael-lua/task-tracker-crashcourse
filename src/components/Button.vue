<template>
  <button @click="onClick()" :style="{ background: color }" class="btn">{{text}}</button> <!-- @click is a directive to v-on:click -->
</template>

<script>
export default {
  name: "Button",
  props: {
    text: String,
    color: String
  },
  methods: {
    onClick() {
      console.log("clicked");
    }
  }
}
</script>