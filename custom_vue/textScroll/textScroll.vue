<template>
  <ul class="text-wrap">
    <li
      @click="pause = !pause"
      v-for="item in dataList"
      :key="item.id"
      :class="{ actItem: actID === item.id }"
      v-show="actID === item.id"
      :style="[
        actID === item.id ? { transform: `translateX(${scrollSize}px)` } : {},
        styles,
      ]"
    >
      {{ item.text }}
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      actID: 0,
      maxID: 0,
      minID: 0,
      scrollSize: 0,
      timer: null,
      blank: 0,
      pause: false,
      stop: false,
    };
  },
  props: {
    dataList: {
      type: Array,
      default: () => [],
    },
    styles: {
      type: Object,
      default: () => ({}),
    },
    ms: {
      type: Number,
      default: () => 20,
    },
    mode: {
      type: String,
      default: () => "single",
    },
  },
  created() {
    if (this.dataList.length > 0) {
      this.dataList.forEach((item) => {
        if (item.id < this.minID) {
          this.minID = item.id;
        }
        if (item.id > this.maxID) {
          this.maxID = item.id;
        }
      });
    }
  },
  mounted() {
    this.blank = document.getElementsByClassName("text-wrap")[0].offsetWidth;
    this.textScroll();
  },
  beforeDestroy() {
    this.stop = true;
  },
  methods: {
    // 公告流动
    textScroll() {
      var w = document.getElementsByClassName("actItem")[0].offsetWidth;
      this.scrollSize = 100;
      const self = this;
      this.timer = setInterval(() => {
        // 暂停
        if (!self.pause) {
          self.scrollSize--;
          if (self.scrollSize <= -w) {
            //   多公告模式
            if (self.mode === "multiple") {
              self.actID =
                self.actID >= self.maxID ? self.minID : self.actID + 1;
              w = document.getElementsByClassName("actItem")[0].offsetWidth;
            }
            // 单公告模式
            self.scrollSize = self.blank;
          }
        }
        // 停止公告
        if (self.stop) {
          clearInterval(self.timer);
          self.timer = null;
        }
      }, this.ms);
    }
  },
};
</script>

<style lang="scss" scoped>
.text-wrap {
  width: 100%;
  height: 100%;
  overflow: hidden;
  display: block;

  li {
    float: left;
    height: 100%;
    white-space: nowrap;
  }
}
</style>