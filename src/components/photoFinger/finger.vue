<template xmlns:v-finger="http://www.w3.org/1999/xhtml">
    <div v-show="isShow"
         :class="[ outFinger, { selectBorder : isEdit}]"  :style="imgStyle" ref="finger" id="finger"
         v-finger:tap="tap"
         v-finger:multipoint-start="multipointStart"
         v-finger:swipe="swipe"
         v-finger:pinch="pinch"
         v-finger:rotate="rotate"
         v-finger:press-move="pressMove.bind(this, 12)"
         v-finger:multipoint-end="multipointEnd"
         v-finger:single-tap="singleTap"
         v-finger:touch-start="touchStart"
         v-finger:touch-move="touchMove"
         v-finger:touch-end="touchEnd"
         v-finger:touch-cancel="touchCancel"
         @click.stop="fingerClick"
         >
      <div v-show="isEdit" >
        <div  class="lt" :style="tipStyle"  @touchstart.stop="deleteElemet($event)"><span class="iconfont icon-guanbi"></span></div>
        <div  ref="rb" class="rb" :style="tipStyle" @touchstart.stop="scaletouchStart" @touchmove.stop="scaletouchMove" @touchend.stop="scaletouchEnd"><span class="iconfont icon-fangda"></span></div>
      </div>
      <img style="width:100%;height:100%;" :src="imgSrc">
    </div>
</template>
<script>
  export default{
    data () {
      return {
        ok: true,
        left: this.clientLeft,
        top: this.clientTop,
        scale: 1,
        angle: 0,
        width: 80,
        height: 80,
        tipScale: 1,
        currentX: 0,
        currentY: 0,
        initScale: 1,
        isShow: true,
        isEdit: true,
        fingerZindex: 1,
        preV: { x: null, y: null },
        pinchStartLen: 0,
        outFinger: 'out-finger',
        isMultiTouch: false,
        currentIndex: this.index,
        posix: {}
      };
    },
    props: {
      imgSrc: {
        require: true
      },
      index: {
        require: true
      },
      isActive: {
        require: true
      },
      clientTop: {
        require: true
      },
      clientLeft: {
        require: true
      },
    },
    computed: {
      imgStyle () {
        var self = this;
        var tempObj = {
          width: self.width + 'px',
          height: self.height + 'px',
          'z-index': self.fingerZindex,
          transform: 'translate3d(' + self.left + 'px, ' + self.top + 'px, 0) scale(' + self.scale + ') rotate(' + self.angle + 'deg)'
        };
        return tempObj;
      },
      tipStyle () {
        let self = this;
        let tipObj = {
          transform: 'scale(' + self.tipScale + ')'
        };
        return tipObj;
      }
    },
    watch: {
      isActive: function (val) {
        this.isEdit = val;
      }
    },
    mounted: function () {

    },
    methods: {
      tap: function () {

      },
      multipointStart: function () {
        var self = this;
        self.initScale = self.scale;
        self.isMultiTouch = true;
      },
      swipe: function (evt) {
      },
      pinch: function (evt) {
        var self = this;
        self.scale = self.initScale * evt.zoom;
        self.tipScale = 1 / self.scale;
        evt.preventDefault();
      },
      rotate: function (evt) {
        var self = this;
        self.angle += evt.angle;
        evt.preventDefault();
      },
      pressMove: function (num, evt) {
        var self = this;
        self.left += evt.deltaX;
        self.top += evt.deltaY;
        evt.preventDefault();
      },
      multipointEnd: function (evt) {
        var self = this;
        evt.preventDefault();
      },
      singleTap: function () {

      },
      touchStart: function () {
        this.$emit('tapSelect', this.currentIndex);
      },
      touchMove: function () {
      },
      touchEnd: function () {
      },
      touchCancel: function () {
      },
      deleteElemet: function (evt) {
        this.isShow = false;
//        evt.preventDefault();
//        this.$emit('deleteElemet', false);
      },
      scaletouchStart: function (evt) {
        this.posix = {
          originWidth: this.width,
          originHeight: this.height,
          offTop: evt.touches[0].pageY,
          offLeft: evt.touches[0].pageX
        }
      },
      scaletouchMove: function (evt) {
        let nowY = evt.touches[0].pageY;
        let nowX = evt.touches[0].pageX;
        //  宽 高 比较取最小/大？
        let innerWidth = nowX - this.posix.offLeft + this.posix.originWidth;
        let innerHeight = nowY - this.posix.offTop + this.posix.originHeight;
        let minNum =  Math.min(innerWidth,innerHeight);
        this.width = this.height = minNum;
        evt.preventDefault();
      },
      scaletouchEnd: function (evt) {

      },
      calculateLen: function (v) {
        return Math.sqrt(v.x * v.x + v.y * v.y);
      },
      fingerClick: function () {
      },
      getRotateAngle: function (v1, v2) {
        var angle = this.getAngle(v1, v2);
        if (this.cross(v1, v2) > 0) {
          angle *= -1;
        }

        return angle * 180 / Math.PI;
      },
      getAngle: function (v1, v2) {
        var mr = this.getLen(v1) * this.getLen(v2);
        if (mr === 0) return 0;
        var r = this.dot(v1, v2) / mr;
        if (r > 1) r = 1;
        return Math.acos(r);
      },
      getLen: function (v) {
        return Math.sqrt(v.x * v.x + v.y * v.y);
      },
      dot: function (v1, v2) {
        return v1.x * v2.x + v1.y * v2.y;
      },
      cross: function(v1, v2) {
        return v1.x * v2.y - v2.x * v1.y;
      }
    }
  };
</script>
<style scoped lang="less" rel="stylesheet/less">
  div.out-finger{
    position: absolute;
    min-width: 20px;
    min-height: 20px;
    box-sizing: border-box;
  }

  .selectBorder {
    border: 1px solid #ffffff;
  }

  .lt{
    width: 24px;
    height: 24px;
    line-height: 24px;
    border-radius: 12px;
    background-color: #4e5051;
    position: absolute;
    left: -12px;
    top: -12px;
    color: white;
    text-align: center;
    .iconfont {
      font-size: 12px;
    }
  }
  .rt{ background:url(delete.png) no-repeat; right:0;  top:0; position:relative; width:7px; height:7px; }
  .lb{ background:url(delete.png) no-repeat; left:10px; bottom:10px; position:relative; width:20px; height:20px;}
  .rb {
    width: 24px;
    height: 24px;
    line-height: 24px;
    border-radius: 12px;
    background-color: rgb(46, 156, 236);
    position: absolute;
    right: -12px;
    bottom: -12px;
    color: white;
    text-align: center;
    .iconfont {
      font-size: 12px;
    }
  }

</style>
