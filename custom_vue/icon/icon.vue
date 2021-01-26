<template>
  <i :class="`iconfont ${name}`" :style="style"></i>
</template>

<script>
export default {
  data() {
    return {
      style: {},
    };
  },
  props: ["name", "size", "color", "margin"],
  created() {
    if (this.size) {
      this.style.fontSize = this.size + "px";
    }
    if (this.color) {
      this.style.color = this.color;
    }
    if (this.margin) {
      this.style.margin = this.margin;
    }
  },
};
</script>