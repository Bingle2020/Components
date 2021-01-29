<template>
  <ul class="number-box">
    <li v-for="(item, index) in boxs" :key="index">
      <ul
        class="num-list"
        :style="{ transform: `translateY(${-(roll[index] + 10) * itemH}px)` }"
      >
        <li
          :class="'item' + index + '-' + i"
          :style="{ 'font-size': size + 'px' }"
          v-for="(item, i) in arr"
          :key="i"
        >
          {{ item }}
        </li>
      </ul>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    boxs: {
      type: Number,
      default: () => 5,
    },
    size: {
      type: Number,
      default: () => 20,
    },
    disNum: {
      type: String,
      default: () => "1234",
    },
  },
  data() {
    return {
      arr: [
        "0",
        "1",
        "2",
        "3",
        "4",
        "5",
        "6",
        "7",
        "8",
        "9",
        0,
        1,
        2,
        3,
        4,
        5,
        6,
        7,
        8,
        9,
      ],
      roll: [],
      itemH: 0,
    };
  },
  created() {
    for (let i = 0, len = this.boxs; i < len; i++) {
      this.roll[i] = 0;
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.itemH = document.getElementsByClassName("item0-0")[0].offsetHeight;
      console.log(this.itemH);
      this.initTransfrom(this.itemH, this.disNum);
    });
  },
  methods: {
    initTransfrom(h, s) {
      if (s.length < this.boxs) {
        s = "0" + String(Number(s) + Math.pow(10, this.boxs - 1)).substr(1);
      }
      if (s.length > this.boxs) {
        return alert("输入的数字长度超出预定显示框!");
      }
      s.split("").forEach((item, i) => {
        this.roll[i] = Number(item);
      });
      // console.log(this.roll);
    },
  },
};
</script>

<style lang="scss" scoped>
%WH {
  width: 100%;
  height: 100%;
}
.number-box {
  @extend %WH;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;

  & > li {
    width: 3rem;
    height: 100%;
    overflow: hidden;
    // background-color: #0d3777;
    background-image: url('../../assets/images/home/content/index/1_06.png');
    background-repeat: no-repeat;
    background-size: 168% 155%;
    background-position: -1.1rem -.9rem;
  }

  .num-list {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    transition: all 3s cubic-bezier(0.13, 0.29, 0.45, 1);

    li {
      border: 0;
    }
  }
}
</style>