<template>
  <span :class="className" :style="styleObject">
    <slot></slot>
    <i class="base-tag__close base-icon-close" v-if="closeable" @click="close"
      >&#xe69e;</i
    >
  </span>
</template>
<script>
export default {
  name: "Base-Tag",
  props: {
    type: {
      type: String,
      default: "",
    },
    effect: {
      type: String,
      default: "",
    },
    size: {
      type: String,
      default: "medium",
    },
    bgColor: {
      type: String,
      default: "",
    },
    fontColor: {
      type: String,
      default: "",
    },
    closeable: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      className: null,
      styleObject: {},
    };
  },
  methods: {
    click() {},
    close() {
      this.$emit("close");
    },
    checkColor(color, type) {
      const colorArray = [
        "red",
        "green",
        "blue",
        "magenta",
        "yellow",
        "chocolate",
        "black",
        "aquamarine",
        "lime",
        "fuchsia",
        "brass",
        "azure",
        "brown",
        "bronze",
        "deeppink",
        "aliceblue",
        "gray",
        "copper",
        "coral",
        "feldspar",
        "orange",
        "orchid",
        "pink",
        "plum",
        "quartz",
        "purple",
        "aliceblue",
        "antiquewith",
        "blanchedalmond",
        "blueviolet",
        "beige",
        "burlywood",
        "bisque",
        "cadetblue",
        "pink",
        "saddlebrown",
        "royalblue",
        "rosybrown",
        "purple",
        "olivedrab",
        "powderblue",
        "peachpuff",
        "papayawhip",
        "paleturquoise",
        "palevioletred",
        "palegreen",
        "navyblue",
        "navajowhite",
        "palegodenrod",
        "violetred",
        "yellowgreen",
        "tomato",
        "turquoise",
        "thistle",
        "springgreen",
        "steelblue",
        "salmon",
        "scarlet",
        "sienna",
        "silver",
        "tan",
        "thistle",
        "turquoise",
        "violet",
        "snow",
        "salmon",
        "scarlet",
        "sienna",
        "silver",
        "tan",
        "thistle",
        "turquoise",
        "violet",
        "chartreuse",
        "firebrick",
        "gold",
        "khaki",
        "mediumslateblue",
        "mediumvioletred",
        "oldlace",
        "maroom",
        "goldenrod",
        "wheat",
        "whitesmoke",
        "orange",
        "moccasin",
        "mistyrose",
        "mintcream",
        "midnightblue",
        "dimgray",
        "darksalmon",
        "slategray",
        "skyblue",
        "sienna",
        "seashell",
        "salmon",
        "seagreen",
        "sandybrown",
        "firebrick",
        "gold",
        "khaki",
        "maroom",
        "goldenrod",
        "wheat",
        "whitesmoke",
        "mediumturquoise",
        "navy",
        "mediumspringgreen",
        "mediumseagreen",
        "mediumpurpul",
        "peru",
        "mediumorchid",
        "mediumblue",
        "mediumaquamarine",
        "maroon",
        "limegreen",
        "lightyellow",
        "lightsteelblue",
        "magenta",
        "lightslateblue",
        "lightslategray",
        "lightskyblue",
        "inen",
        "lightseagreen",
        "lightsalmon",
        "lightpink",
        "plum",
        "lightgray",
        "lightgreen",
        "lightgodenrodyellow",
        "indianred",
        "lavender",
        "lightblue",
        "lavenderblush",
        "lightcoral",
        "lightcyan",
        "lightgodenrod",
        "hotpink",
        "greenyellow",
        "lemonchiffon",
        "lawngreen",
        "khaki",
        "deepskyblue",
        "honeydew",
        "golenrod",
        "forestgreen",
        "gostwhite",
        "greenyellow",
        "gainsboro",
        "firebrick",
        "dodgerblue",
        "darkturquoise",
        "darkslateblue",
        "darkslategray",
        "darkseagreen",
        "darkred",
        "darkorenge",
        "darkslateblue",
        "darkviolet",
        "floralwhite",
        "cyan",
        "bisque darkgray",
        "cornsilk",
        "darkolivegreen",
        "darkgoldenrod",
        "darkblue",
        "darkcyan",
        "darkgreen",
        "darkhaki",
        "ivory",
        "darkmagenta",
        "darkgray",
        "cornfloewrblue",
        "cornfloewrblue",
        "darkviolet",
        "floralwhite",
        "darkslategray",
        "darkseagreen",
        "darkred",
        "darkorchid",
        "darkorenge",
        "darkslateblue",
      ];

      const pattarn = new RegExp("^#([0-9a-fA-F]{6}|[0-9a-fA-F]{3})$");

      if (color.indexOf("#") !== -1) {
        if (pattarn.test(color)) {
          if (type === "bg")
            this.styleObject.backgroundColor = this.bgColor ? this.bgColor : "";
          else this.styleObject.color = this.fontColor ? this.fontColor : "";
        } else {
          console.log("color format isn't right!");
        }
      } else {
        if (/^[A-Za-z]+$/.test(color) && colorArray.indexOf(color) !== -1) {
          if (type === "bg")
            this.styleObject.backgroundColor = this.bgColor ? this.bgColor : "";
          else this.styleObject.color = this.fontColor ? this.fontColor : "";
        } else {
          console.log("bgColor format isn't right!");
        }
      }
    },
  },
  created() {
    this.className = this.effect
      ? `base-tag base-tag--${this.effect}`
      : "base-tag";
    this.className = this.type
      ? `${this.className} base-tag--${this.type}`
      : this.className;
    this.className = this.size
      ? `${this.className} base-tag--${this.size}`
      : this.className;

    if (this.bgColor) this.checkColor(this.bgColor, "bg");
    if (this.fontColor) this.checkColor(this.fontColor, "font");
  },
};
</script>
<style scoped>
@font-face {
  font-family: "iconfont";
  src: url("../fonts/iconfont.eot");
  src: url("../fonts/iconfont.eot?#iefix") format("embedded-opentype"),
    url("../fonts/iconfont.woff2") format("woff2"),
    url("../fonts/iconfont.woff") format("woff"),
    url("../fonts/iconfont.ttf") format("truetype"),
    url("../fonts/iconfont.svg#iconfont") format("svg");
}

.base-icon-close {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.base-tag {
  background-color: #ecf5ff;
  display: inline-block;
  height: 32px;
  padding: 0 10px;
  line-height: 30px;
  font-size: 12px;
  color: #409eff;
  border: 1px solid #d9ecff;
  border-radius: 4px;
  box-sizing: border-box;
  white-space: nowrap;
}

.base-tag .base-tag__close {
  color: #409eff;
}

.base-tag .base-tag__close:hover {
  color: #fff;
  background-color: #409eff;
}

.base-tag .base-icon-close {
  border-radius: 50%;
  text-align: center;
  position: relative;
  cursor: pointer;
  font-size: 16px;
  height: 16px;
  width: 16px;
  line-height: 16px;
  vertical-align: middle;
  top: -1px;
  right: -5px;
}

.base-tag.base-tag--info {
  background-color: #f4f4f5;
  border-color: #e9e9eb;
  color: #909399;
}
.base-tag.base-tag--info .base-tag__close {
  color: #909399;
}
.base-tag.base-tag--info .base-tag__close:hover {
  color: #fff;
  background-color: #909399;
}

.base-tag.base-tag--success {
  background-color: #f0f9eb;
  border-color: #e1f3d8;
  color: #67c23a;
}
.base-tag.base-tag--success .base-tag__close {
  color: #67c23a;
}
.base-tag.base-tag--success .base-tag__close:hover {
  color: #fff;
  background-color: #67c23a;
}

.base-tag.base-tag--warning {
  background-color: #fdf6ec;
  border-color: #faecd8;
  color: #e6a23c;
}
.base-tag.base-tag--warning .base-tag__close {
  color: #e6a23c;
}
.base-tag.base-tag--warning .base-tag__close:hover {
  color: #fff;
  background-color: #e6a23c;
}

.base-tag.base-tag--danger {
  background-color: #fef0f0;
  border-color: #fde2e2;
  color: #f56c6c;
}

.base-tag.base-tag--danger.is-hit {
  border-color: #f56c6c;
}
.base-tag.base-tag--danger .base-tag__close {
  color: #f56c6c;
}
.base-tag.base-tag--danger .base-tag__close:hover {
  color: #fff;
  background-color: #f56c6c;
}

.base-tag--dark {
  background-color: #409eff;
  color: #fff;
}
.base-tag--dark .base-tag__close {
  color: #fff;
}
.base-tag--dark .base-tag__close:hover {
  color: #fff;
  background-color: #66b1ff;
}

.base-tag--dark.base-tag--info {
  background-color: #909399;
  border-color: #909399;
  color: #fff;
}
.base-tag--dark.base-tag--info .base-tag__close {
  color: #fff;
}
.base-tag--dark.base-tag--info .base-tag__close:hover {
  color: #fff;
  background-color: #a6a9ad;
}

.base-tag--dark.base-tag--success {
  background-color: #67c23a;
  border-color: #67c23a;
  color: #fff;
}
.base-tag--dark.base-tag--success .base-tag__close {
  color: #fff;
}
.base-tag--dark.base-tag--success .base-tag__close:hover {
  color: #fff;
  background-color: #85ce61;
}

.base-tag--dark.base-tag--warning {
  background-color: #e6a23c;
  border-color: #e6a23c;
  color: #fff;
}
.base-tag--dark.base-tag--warning .base-tag__close {
  color: #fff;
}
.base-tag--dark.base-tag--warning .base-tag__close:hover {
  color: #fff;
  background-color: #ebb563;
}

.base-tag--dark.base-tag--danger {
  background-color: #f56c6c;
  border-color: #f56c6c;
  color: #fff;
}
.base-tag--dark.base-tag--danger .base-tag__close {
  color: #fff;
}
.base-tag--dark.base-tag--danger .base-tag__close:hover {
  color: #fff;
  background-color: #f78989;
}

.base-tag--plain {
  background-color: #fff;
  border-color: #b3d8ff;
  color: #409eff;
}
.base-tag--plain .base-tag__close {
  color: #409eff;
}
.base-tag--plain .base-tag__close:hover {
  color: #fff;
  background-color: #409eff;
}

.base-tag--plain.base-tag--info {
  background-color: #fff;
  border-color: #d3d4d6;
  color: #909399;
}
.base-tag--plain.base-tag--info .base-tag__close {
  color: #909399;
}
.base-tag--plain.base-tag--info .base-tag__close:hover {
  color: #fff;
  background-color: #909399;
}

.base-tag--plain.base-tag--success {
  background-color: #fff;
  border-color: #c2e7b0;
  color: #67c23a;
}
.base-tag--plain.base-tag--success .base-tag__close {
  color: #67c23a;
}
.base-tag--plain.base-tag--success .base-tag__close:hover {
  color: #fff;
  background-color: #67c23a;
}

.base-tag--plain.base-tag--warning {
  background-color: #fff;
  border-color: #f5dab1;
  color: #e6a23c;
}
.base-tag--plain.base-tag--warning .base-tag__close {
  color: #e6a23c;
}
.base-tag--plain.base-tag--warning .base-tag__close:hover {
  color: #fff;
  background-color: #e6a23c;
}

.base-tag--plain.base-tag--danger {
  background-color: #fff;
  border-color: #fbc4c4;
  color: #f56c6c;
}
.base-tag--plain.base-tag--danger .base-tag__close {
  color: #f56c6c;
}
.base-tag--plain.base-tag--danger .base-tag__close:hover {
  color: #fff;
  background-color: #f56c6c;
}

.base-tag--medium {
  height: 28px;
  line-height: 26px;
}

.base-tag--medium .base-icon-close {
  transform: scale(0.8);
}

.base-tag--small {
  height: 24px;
  padding: 0 8px;
  line-height: 22px;
}

.base-tag--small .base-icon-close {
  transform: scale(0.8);
}

.base-tag--mini {
  height: 20px;
  padding: 0 5px;
  line-height: 19px;
}

.base-tag--mini .base-icon-close {
  margin-left: -3px;
  transform: scale(0.7);
}
</style>
