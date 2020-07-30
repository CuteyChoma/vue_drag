<template>
  <div id="dragContainer">
    <div
      class="hidden-sm-and-up"
      id="moveDiv"
      @mousedown="down"
      @touchstart="down"
      @mousemove="move"
      @touchmove.prevent.stop="move"
      @mouseup="end"
      @touchend="end"
      @click="tabswith"
    >
      <span>拖拽</span>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  props: {},
  data() {
    return {
      flags: false,
      position: { x: 0, y: 0 },
      nx: "",
      ny: "",
      dx: "",
      dy: "",
      xPum: "",
      yPum: "",
    };
  },
  methods: {
    tabswith() {
      this.isswitcha = !this.isswitcha;
    },
    down() {
      this.flags = true;
      var touch;
      if (event.touches) {
        console.log(event.touches,"event.touches");
        touch = event.touches[0];
      } else {
        touch = event;
      }
      this.position.x = touch.clientX;
      this.position.y = touch.clientY;
      this.dx = moveDiv.offsetLeft;
      this.dy = moveDiv.offsetTop;
      // console.log("this.dx" ,this.dx );
      // console.log("this.dy" ,this.dy );
    },
    move() {
      if (this.flags) {
        var touch;
        if (event.touches) {
          touch = event.touches[0];
        } else {
          touch = event;
        }
        this.nx = touch.clientX - this.position.x;
        this.ny = touch.clientY - this.position.y;
        this.xPum = this.dx + this.nx;
        this.yPum = this.dy + this.ny;
        if (this.xPum <= 0) {
          this.xPum = 0;
        } else if (
          this.xPum >=
          document.documentElement.clientWidth - moveDiv.offsetWidth
        ) {
          this.xPum =
            document.documentElement.clientWidth - moveDiv.offsetWidth;
        }
        if (this.yPum <= 0) {
          this.yPum = 0;
        } else if (
          this.yPum >=
          document.documentElement.clientHeight - moveDiv.offsetHeight
        ) {
          this.yPum =
            document.documentElement.clientHeight - moveDiv.offsetHeight;
        }
        moveDiv.style.left = this.xPum + "px";
        moveDiv.style.top = this.yPum + "px";
        window.addEventListener("touchmove", () => event.preventDefault(), {
          passive: false,
        });
      }
    },
    end() {
      this.flags = false;
    },
  },
};
</script>
<style scoped lang='stylus' >
#dragContainer {
  width: 100%;
  height: 100%;
  .hidden-sm-and-up{
    touch-action: none;
    webkit-overflow-scrolling: touch;
    position: fixed;
    top: 120px;
    left: 12px;
    background:rgba(255,0,0,0.5);
    width:50px;
    height:50px;
    line-height:50px;
    text-align:center;
    font-size:15px;
    color:#ff0;

  }
}
</style>