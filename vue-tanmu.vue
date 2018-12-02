<template>
  <div id="tanmuWrapper" ref="tanmuWrapper" >
  </div>
</template>
<script>
import Component from "vue-class-component";
const bodyWidth = window.document.body.clientWidth;

export default {
  name: "jrvm-countdown",
  data() {
    return {
      current: -1
    };
  },
  props: {
    data: Array,
    line: {
      // 行数
      default: 3,
      type: Number
    },
    rate: {
      // 速度单位s
      default: 1,
      type: Number
    },
    onRandom: {
      default: false,
      type: Boolean
    },
    t: {
      // 速度单位s
      default: 10,
      type: Number
    },
    loop: {
      default: true,
      type: Boolean
    }
  },
  mounted() {
    this.move();
  },
  methods: {
    move() {
      this.createDom();
      setInterval(() => {
        this.createDom();
      }, this.rate * 1000);
    },
    createDom() {
      const selfDom = this.$refs.tanmuWrapper;
      const div = window.document.createElement("div");
      const nowDiv = selfDom.querySelectorAll(".barrageItem");
      let divW = 10;
      let clientHtg = this.$refs.tanmuWrapper.clientHeight + "px" || "200px";
      let y = this.onRandom
        ? `${window.parseInt((bodyWidth * 100) / div.clientWidth)}%`
        : clientHtg;

      if (nowDiv.length === Math.ceil(4.5 / this.rate)) {
        selfDom.removeChild(nowDiv[0]);
      }
      this.current += 1;
      if (this.loop) {
        if (this.current === this.data.length) {
          this.current = 0;
        }
      } else {
        if (this.current > this.data.length - 1) {
          clearTimeout(timer);
          return false;
        }
      }
      div.innerHTML = `${this.data[this.current]}`;
      div.classList.add("barrageItem");
      selfDom.appendChild(div);
      // const divH = div.clientHeight * Math.floor(Math.random() * this.line);
      div.style.width = `${div.clientWidth}px`;
      div.style.webkitTransform = `translate3d(${divW}px, ${y}, 0)`;
      var timer = setTimeout(() => {
        div.style.webkitTransform = `translate3d(${divW}px, -100%, 0)`;
        div.classList.add("move");
      }, this.t);
    }
  }
};
</script>

<style lang="scss">
@import "./../css/mixin.scss";
#tanmuWrapper {
  width: 100%;
  height: $rem400;
  position: relative;
  top: 0;
  left: 0;
  width: 100%;
  overflow-y: hidden;
  .barrageItem {
    width: auto;
    text-align: center;
    height: 30px;
    background-color: rgba(0, 59, 147, 0.7);
    font-size: 11px;
    color: #feed64;
    display: inline-block;
    line-height: 30px;
    padding: 0px 4px;
    border-radius: 30px;
    position: absolute;
    white-space: nowrap;
    &.move {
      transition: transform 4.5s linear;
    }
  }
}
</style>