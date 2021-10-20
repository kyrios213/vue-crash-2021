<template>
  <button @click="Click" class="btn" :style="{ background: color }">
    {{ text }}
  </button>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: String,
    color: String,
  },
  methods: {
    Click() {
      console.log("Clicked");
    },
  },
};
</script>
