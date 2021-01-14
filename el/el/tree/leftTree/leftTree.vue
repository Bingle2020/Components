<template>
  <div class="tree-node">
    <el-checkbox
      class="all-sel"
      :indeterminate="!selAll"
      v-model="selAll"
      @change="selAllChange"
      >全选</el-checkbox
    >
    <el-tree
      class="single-sel"
      :data="data"
      show-checkbox
      node-key="id"
      ref="tree"
      :props="defaultProps"
      @check="selSingleChange"
    >
    </el-tree>
  </div>
</template>

<script>
import { recursive } from "@/util/index";
export default {
  data() {
    return {
      selAll: false,
      defaultProps: {
        children: "children",
        label: "label",
      },
      ids: [],
    };
  },
  props: {
    data: {
      type: Array,
      default: () => [],
    },
  },
  created() {
    this.ids = recursive(this.data, "id");
  },
  methods: {
    // 设置tree勾选
    setTree(arr) {
      this.$refs.tree.setCheckedKeys(arr);
      this.selAll = arr.length >= this.ids.length ? true : false;
    },
    // 全选
    selAllChange(val) {
      let result = val ? this.ids : [];
      this.$refs.tree.setCheckedKeys(result);
      let seled = this.$refs.tree.getCheckedKeys(true);
      this.$emit("change", seled);
    },
    // 单选
    selSingleChange() {
      let result = this.$refs.tree.getCheckedKeys();
      this.selAll = result.length >= this.ids.length ? true : false;
      let seled = this.$refs.tree.getCheckedKeys(true);
      this.$emit("change", seled);
    },
  },
};
</script>

<style lang="scss" scoped>
.tree-node {
  width: 100%;
  height: 100%;
  .all-sel {
    margin-bottom: 15px;
    padding: 10px 0 10px 9px;
    height: 43px;
    width: 100%;
    box-sizing: border-box;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    /deep/ .el-checkbox__inner {
      width: 23px;
      height: 22px;
      background-color: #00cccb;
      &::before {
        height: 4px;
        top: 8px;
      }
      &::after {
        transform: rotate(45deg) scaleY(1.2);
        border-width: 2px;
        left: 9px;
        top: 4px;
      }
    }
    /deep/ .el-checkbox__label {
      width: 150px;
      height: 100%;
      line-height: 22px;
      font-size: 22px;
      color: #00cccb;
    }
  }
  .single-sel {
    background-color: transparent;
    /deep/ .el-tree-node {
      .el-tree-node__content {
        background-color: transparent!important;
      }
    }
    /deep/ .el-tree-node__content {
      height: 50px;
      color: #fff;
      &:hover {
        background-color: transparent;
        color: #00cccb;
      }
    }
    /deep/ .is-current {
      .el-tree-node__content {
        background-color: transparent;
        // color: #00cccb;
      }
    }
    /deep/ .is-expanded {
      .el-tree-node__content {
        background-color: transparent;
        // color: #00cccb;
      }
    }
    /deep/ .el-tree-node__label {
      font-size: 16px;
    }
    /deep/ .el-tree-node__expand-icon {
      font-size: 16px;
    }
    /deep/ .el-checkbox {
      font-size: 25px;
    }
    /deep/ .el-checkbox__inner {
      width: 18px;
      height: 18px;
      // background-color: transparent;
      &::before {
        height: 3px;
        top: 7px;
      }
      &::after {
        width: 4px;
        height: 8px;
        left: 6px;
        top: 2px;
        transform: rotate(45deg) scale(1.1);
      }
    }
    /deep/ .is-checked {
      .el-tree-node__content {
        color: #00cccb;
      }
      .el-checkbox__inner {
        background-color: #00cccb;
        border-color: #00cccb;
        &::before {
          color: #fff;
        }
        &::after {
          color: #fff;
        }
      }
    }
    /deep/ .is-indeterminate {
      .el-checkbox__inner {
        background-color: #00cccb;
        border-color: #00cccb;
        &::before {
          color: #fff;
        }
        &::after {
          color: #fff;
        }
      }
    }
  }
}
</style>