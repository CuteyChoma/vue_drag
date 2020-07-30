<template>
  <div id="dargerContainer">
    <div class="leftContainer">
      <button class="clickBtn" @click="showChart"  >
        <span>点击拖拽</span>
      </button>
      <div
        class="hidden-sm-and-up"
        id="moveDiv"
        v-show="isShowDraggerBox"
        @mousedown="down"
        @touchstart="down"
        @mousemove="move"
        @touchmove.prevent.stop="move"
        @mouseup="end"
        @touchend="end"
        @click="tabswith"
        :style= "`background-color:${color};position:absolute;left:${position.x}px;top:${position.y}px; `"
      >
        <span>拖我</span>
      </div>
    </div>
    <div class="centerContainer"></div>
    <div class="rightContainer"></div>
  </div>
</template>

<script>
export default {
  components: {},
  props: {},
  data() {
    return {
      isShowDraggerBox: false,
      flags: false,
      position: { x: 0, y: 0 },
      nx: "",
      ny: "",
      dx: "",
      dy: "",
      xPum: "",
      yPum: "",
      color: "orange"
    };
  },
  computed: {},
  watch: {},
  methods: {
    tabswith(){
      this.isswitcha = !this.isswitcha;

    },
    showChart() {
      this.isShowDraggerBox = !this.isShowDraggerBox;
      // console.log("event",event);
      this.position.x = event.clientX;
      this.position.y = event.clientY;
    },
    down() {
      this.flags = true;
      var touch;
      if (event.touches) {
        console.log(event.touches, "event.touches01");
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
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {},
  //生命周期-创建之前
  beforeCreated() {},
  //生命周期-挂载之前
  beforeMount() {},
  //生命周期-更新之前
  beforUpdate() {},
  //生命周期-更新之后
  updated() {},
  //生命周期-销毁之前
  beforeDestory() {},
  //生命周期-销毁完成
  destoryed() {},
  //如果页面有keep-alive缓存功能，这个函数会触发
  activated() {},
};
</script>
<style scoped lang='stylus' >
/* @import url(); 引入css类 */
#dargerContainer {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 850px;
  display: flex;

  .leftContainer {
    width: 20%;
    height: 100%;

    // background-color #ff0
    .clickBtn {
      width: 70px;
      height: 30px;
      line-height: 30px;
      text-align: center;
      color: orange;
      border: 1px solid red;
    }

    .hidden-sm-and-up {
      width: 50px;
      height: 50px;
      background-color: pink;
      line-height: 50px;
      text-align: center;
      touch-action: none;
      webkit-overflow-scrolling: touch;
      font-size: 15px;
      color: #ff0;
    }
  }

  .centerContainer {
    width: 50%;
    height: 100%;
    // background-color #f05
  }

  .rightContainer {
    width: 30%;
    height: 100%;
    // background-color #f90
  }
}
</style>