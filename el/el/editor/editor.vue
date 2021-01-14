<template>
  <div class="editor-wrap">
    <div id="toolbar"></div>
    <div id="content"></div>
  </div>
</template>

<script>
// https://doc.wangeditor.com/pages/11-%E8%87%AA%E5%AE%9A%E4%B9%89%E6%89%A9%E5%B1%95%E8%8F%9C%E5%8D%95/01-%E5%BF%AB%E9%80%9F%E6%89%A9%E5%B1%95%E4%B8%80%E4%B8%AA%E8%8F%9C%E5%8D%95.html
import wangEditor from "wangeditor";
export default {
  props: {
    // 默认所有菜单
    menus: {
      type: Array,
      default: () => [
        // "head",
        // "bold",
        "fontName",
        "fontSize",
        // "italic",
        // "underline",
        // "strikeThrough",
        "indent",
        // "lineHeight",
        "foreColor",
        // "backColor",
        // "link",
        // "list",
        // "todo",
        // "justify",
        // "quote",
        // "emoticon",
        // "image",
        // "video",
        // "table",
        // "code",
        // "splitLine",
        // "undo",
        // "redo",
      ],
    },
    // 剔除的菜单
    excludeMenus: {
      type: Array,
      default: () => [],
    },
    // 颜色
    colors: {
        type: Array,
        default: () => []
    },
    // 字体
    fontNames: {
        type: Array,
        default: () => []
    },

  },
  data() {
    return {
      editor: null,
      editorData: "",
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      const editor = new wangEditor("#toolbar", "#content");
      editor.config.menus = this.menus.length > 0 ? this.menus : editor.config.menus;
      editor.config.colors = this.colors.length > 0 ? this.colors : editor.config.colors;
      editor.config.fontNames = this.fontNames.length > 0 ? this.fontNames : editor.config.fontNames;
      editor.config.onchange = (val) => {
        this.editorData = val;
      };
      editor.create();
      this.editor = editor;
    },
    getDatas() {
      //   console.log(this.editor.txt.html());
      //   this.$emit('datas',this.editorData);
      return this.editorData;
    },
  },
  beforeDestroy() {
    this.editor.destroy();
    this.editor = null;
  },
};
</script>

<style lang="scss" scoped>
.editor-wrap {
  width: 100%;
  height: 100%;
  #toolbar {
    border: 1px solid #ccc;
  }
  #content {
    border: 1px solid #ccc;
    height: 368px;
  }
}
</style>