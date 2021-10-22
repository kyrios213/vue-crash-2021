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
      this.$emit("btn-click");
    },
  },
};
</script>
