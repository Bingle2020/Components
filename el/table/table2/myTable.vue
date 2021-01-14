<template>
  <div class="table-wrap" ref="table" :style="{}">
    <span class="title">
      <img :src="logoImg" class="logoimg" v-if="logoImg" />
      <icon
        v-if="logoIcon"
        class="logoicon"
        :name="logoIcon[0]"
        :size="logoIcon[1]"
        :color="logoIcon[2]"
      ></icon>
      {{ title }}
    </span>
    <el-divider v-if="splitline"></el-divider>
    <!-- 数据列表 -->
    <div class="table-box">
      <el-table
        :data="sources"
        :header-row-style="{ backgroundColor: 'transparent' }"
        :header-cell-style="{
          backgroundColor: 'transparent',
          textAlign: 'center',
          color: '#00cdcb',
          borderColor: '#6980808c',
        }"
        :row-style="{ backgroundColor: 'transparent' }"
        :cell-style="{
          backgroundColor: 'transparent',
          textAlign: 'center',
          color: '#00cdcb',
          borderColor: '#6980808c',
        }"
        height="100%"
        border
        stripe
        style="width: 100%"
      >
        <el-table-column
          v-for="(item, i) in columns"
          :prop="item.key"
          :label="item.title"
          :width="item.width"
          :key="i"
        >
        </el-table-column>
        <el-table-column
          v-if="actions"
          :width="actions.width"
          fixed="right"
          :label="actions.title"
        >
          <template slot-scope="scope">
            <!-- <el-button
              v-for="(item, i) in actions.actions"
              @click="handleClick(item.fn, scope.row)"
              :type="item.type"
              :icon="item.icon"
              size="small"
              round
              :key="i"
            >
              {{ item.edit }}
            </el-button> -->
            <icon
              v-for="(item, i) in actions.actions"
              @click.native="handleClick(item.fn, scope.row)"
              class="operateLogo"
              :name="item.icon"
              :size="item.size"
              :margin="item.margin"
              :color="item.color"
              :key="i"
            ></icon>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <!-- 分页 -->
    <div class="splitPage" v-if="splitPage">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage1"
        :page-sizes="pageSizes"
        :page-size="pageSize"
        :layout="layout"
        :total="sources.length"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
import icon from "@/components/icon/icon";
export default {
  components: {
    icon,
  },
  data() {
    return {
      currentPage1: 1,
    };
  },
  props: {
    logoIcon: {
      type: Array,
      default: () => undefined,
    },
    logoImg: {
      type: String,
      default: () => undefined,
    },
    high: {
      type: String,
      default: () => "auto",
    },
    title: String,
    sources: {
      type: Array,
      default: () => [],
    },
    columns: {
      type: Array,
      default: () => [],
    },
    actions: {
      type: Object,
      default: () => undefined,
    },
    splitPage: {
      type: Boolean,
      default: () => false,
    },
    pageSize: {
      type: Number,
      default: () => 50,
    },
    pageSizes: {
      type: Array,
      default: () => [50, 100, 150, 200],
    },
    splitline: {
      type: Boolean,
      default: () => false,
    },
    layout: {
      type: String,
      default: () => "prev, pager, next, jumper, sizes, total",
    },
  },
  computed: {},
  methods: {
    handleClick(fn, row) {
      let obj = {
        fn,
        data: row,
      };
      this.$emit("clickFn", obj);
    },
    handleSizeChange(val) {
      this.$emit("sizeChange", val);
    },
    handleCurrentChange(val) {
      this.$emit("currentChange", val);
    },
  },
};
</script>

<style lang="scss" scoped>
.el-table {
  background-color: transparent;
  &::before {
    background-color: #6980808c;
  }
}
.el-table--border {
  border: 1px solid #6980808c;
  border-bottom: 0;
  border-right: 0;
}
.el-table--border::after {
  background-color: #6980808c;
}
.el-table__fixed-right {
  /deep/ &::before {
    background-color: #6980808c;
  }
}
/deep/ .hover-row {
  background-color: #1E3636!important;
}
.table-wrap {
  position: relative;
  height: 100%;
  padding: 5px 10px;
  border-radius: 5px;
  overflow: hidden;
  box-sizing: border-box;
  .title {
    padding: 10px 12px 15px 0;
    width: 150px;
    height: 25px;
    line-height: 25px;
    color: #00cdcb;
    font-size: 22px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    letter-spacing: 2px;
    .logoimg {
      display: inline-block;
      width: 25px;
      height: 25px;
      margin-right: 10px;
    }
    .logoicon {
      margin-right: 10px;
    }
  }
  .table-box {
    height: calc(100% - 102px);
    .operateLogo {
      cursor: pointer;
    }
  }
  .splitPage {
    margin: 10px 10px 10px 0;
    position: absolute;
    bottom: 0;
    .el-pagination {
      padding-left: 0;
    }
  }
}
// 分页
.el-pagination {
  /deep/ button {
    background-color: transparent;
  }
  /deep/ .el-pager {
    border: 1px solid #00cdcb;
    border-radius: 15px;
    li {
      background-color: transparent;
    }
  }
  /deep/ .el-input__inner {
    background-color: transparent;
    border: 1px solid #00cdcb;
    border-radius: 15px;
  }
}
</style>