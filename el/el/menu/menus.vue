<template>
  <ul class="menu-list">
    <template v-for="item in datas">
      <!-- 一级菜单 -->
      <li :class="{active: item.id === activeId}" :key="item.id" @click="collapse1(item)">
        <div class="img">
          <img
            v-if="item.img"
            :src="item.img"
            class="auto-img"
            alt="无法显示"
          />
          <icon v-else class="logo" :name="item.icon"></icon>
        </div>
        <div class="nav">
          {{ item.title }}
          <div class="img" v-show="item.child.length > 0">
            <i v-show="!item.expand" class="el-icon-arrow-down icons"></i>
            <i v-show="item.expand" class="el-icon-arrow-up icons"></i>
          </div>
        </div>
      </li>
      <transition name="el-zoom-in-top" :key="item.id + new Date().getTime()">
        <template v-if="item.expand && item.child.length > 0">
          <ul class="menu-list2">
            <template v-for="child in item.child">
              <!-- 二级菜单 -->
              <li :class="{active: child.id === activeId}" :key="child.id" @click="collapse2(item.id, child)">
                <div class="nav-name">
                  {{ child.title }}
                  <div class="img" v-show="child.child.length > 0">
                    <i
                      v-show="!child.expand"
                      class="el-icon-arrow-down icons"
                    ></i>
                    <i v-show="child.expand" class="el-icon-arrow-up icons"></i>
                  </div>
                </div>
              </li>
              <transition
                name="el-zoom-in-top"
                :key="child.id + new Date().getTime()"
              >
                <template v-if="child.expand && child.child.length > 0">
                  <ul class="menu-list3">
                    <!-- 三级菜单 -->
                    <li
                      :class="{active: v.id === activeId}"
                      v-for="v in child.child"
                      :key="v.id"
                      @click="linkTo(v)"
                    >
                      <div class="nav-name">{{ v.title }}</div>
                    </li>
                  </ul>
                </template>
              </transition>
            </template>
          </ul>
        </template>
      </transition>
    </template>
  </ul>
</template>

<script>
import icon from "@/components/icon/icon";
export default {
  components: {
    icon
  },
  data() {
    return {
      datas: [],
      activeId: null
    };
  },
  props: {
    source: {
      type: Array,
      default: () => [],
    },
  },
  created() {
    this.datas = [...this.source];
  },
  computed: {
    data() {
      return this.datas;
    },
  },
  methods: {
    // 一级折叠
    collapse1(item) {
      if (item.child.length <= 0) {
        this.activeId = item.id;
        this.$emit("toLink", item.path);
        return;
      }
      let expand = !item.expand;
      this.datas = this.datas.map((v) => {
        if (v.id === item.id) {
          v.expand = expand;
        }
        return v;
      });
    },
    // 二级折叠
    collapse2(id, items) {
      if (items.child.length <= 0) {
        this.activeId = items.id;
        this.$emit("toLink", items.path);
        return;
      }
      let ids = items.id;
      let expand = !items.expand;
      this.datas = this.datas.map((elem) => {
        if (elem.id === id) {
          elem.child = elem.child.map((item) => {
            if (item.id === ids) {
              item.expand = expand;
            }
            return item;
          });
        }
        return elem;
      });
    },
    // 直接跳转
    linkTo(v) {
      this.activeId = v.id;
      this.$emit("toLink", v.path);
    },
  },
};
</script>

<style lang="scss" scoped>
.menu-list {
  width: 100%;
  height: 100%;
  color: #00cdcb;
  overflow-x: hidden;
  overflow-y: auto;
  & > li:first-child {
    margin-top: 0;
  }
  .active {
    color: #ffde25;
  }
}
.menu-list2,
.menu-list3 {
  width: 80%;
  margin-left: 20%;
  li {
    margin: 10px 0;
    height: 30px;
    background-size: 100% 60px;
    background-position: -3px -5px;
  }
}
li {
  height: 47px;
  margin: 15px 0;
  background-image: url("../../assets/images/home/logos/tabBg.png");
  background-repeat: no-repeat;
  background-size: 100% 49px;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  cursor: pointer;
}
.img {
  width: 25px;
  height: 25px;
  margin: 10px;
}
.nav {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
.nav-name {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  margin-left: 12px;
}
.icons {
  display: inline-block;
  width: 100%;
  height: 100%;
  line-height: 28px;
  font-size: 15px;
}
.logo {
  display: block;
  width: 100%;
  height: 100%;
  line-height: 26px;
  font-size: 25px;
  text-align: center;
}
</style>