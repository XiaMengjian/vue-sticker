<template>
  <div id="app" class="page-wrapper" @click.self="clickOutArea">
    <ul v-clickoutside="close">
      <li v-for="(url, index) in fingerDatas">
        <Finger :imgSrc="url.imgUrl" :index="index" :isActive="url.isActive" @deleteElemet="parentDelete" @tapSelect="parentTapSelect" :clientTop="clientTop" :clientLeft="clientLeft"></Finger>
      </li>
    </ul>
    <div style="margin: 0 auto" @click="pop">
      <h1>弹框</h1>
    </div>
    <Pop v-model="popupVisible" position="bottom"  class="mint-popup-4"  @input="currentValue" :closeOnClickModal="true">
      <div class="control">
        <div @click="cancelImg" class="iconfont icon-guanbi"></div>
        <p>选择贴纸</p>
        <div @click="addImg" class="iconfont icon-duihao" style="color: #34cf66"></div>
      </div>
      <swipe :auto="0"  @swipeFinish="swipedOrClose">
        <swipe-item  v-for="(item, index) in imgUrls">
          <ImgTable :imgSrcs="item"
                    @selectIndex="addImgTouchLi"
                    :isSwipeFinish="isSwiped"></ImgTable>
        </swipe-item>
      </swipe>
    </Pop>
  </div>
</template>

<script>
  import Finger from './components/photoFinger/finger.vue';
  import Pop from './components/photoFinger/popup/popup.vue';
  import Swipe from './components/photoFinger/swipe/swipe.vue';
  import SwipeItem from './components/photoFinger/swipe/swipe-item.vue';
  import ImgTable from './components/photoFinger/grid/table.vue';

  export default {
    name: 'app',
    data () {
      return {
        selectImgUrl: '',
        popupVisible: false,
        imgUrls: [
          [ {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/1.png', select: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/2.png', select: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/3.png', select: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/4.png', select: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/5.png', select: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/6.png', select: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/8.png', select: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/9.png', select: false}
          ],
          [ {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/7.png', select: false}
          ]
        ],
        fingerData: { imgUrl: "", isActive: false},
        fingerDatas: [],
        currentSelectFinger: 0,
        isShown: true,
        isSwiped: false,
        clientTop: 0,
        clientLeft: 0
      };
    },
    mounted: function () {
      this.clientLeft = window.innerWidth / 2 - 30;
      this.clientTop =  window.innerHeight / 3 - 30;
    },
    methods: {
      pop: function () {
        this.popupVisible = !this.popupVisible;
      },
      close: function () {

      },
      currentValue: function (val) {
        this.swipedOrClose();
      },
      addImgTouchLi: function (index, url) {
        this.selectImgUrl = url;
      },
      addImg: function () {
        if (this.selectImgUrl.length > 0) {
          this.reductionFingerData();
          let data = { imgUrl: this.selectImgUrl, isActive: true};
          this.fingerDatas.push(data);
        }
        this.popupVisible = false;
      },
      cancelImg: function () {
        this.popupVisible = false;
      },
      parentDelete: function (isShown) {
        this.fingerDatas.splice(this.currentSelectFinger, 1);
      },
      swipedOrClose: function () {
        this.isSwiped = !this.isSwiped;
      },
      tapActive: function (index) {
        alert(index);
      },
      clickOutArea: function () {
        this.reductionFingerData();
      },
      reductionFingerData: function () {
        this.fingerDatas.forEach(function (value) {
          value.isActive = false;
        });
      },
      parentTapSelect: function (index) {
        this.reductionFingerData();
        this.currentSelectFinger = index;
        this.fingerDatas[index].isActive = true;
      }
    },
    components: {
      Finger,
      Pop,
      Swipe,
      SwipeItem,
      ImgTable
    }
  };
</script>

<style>
  @import "assets/common/css/reset.css";
  @import "assets/common/icon/iconfont.css";
  div.finger {
    position: absolute;
  }

  .page-wrapper{
    height: 100%;
    display: flex;
    flex-direction: column;
  }

  h1 {
    font-size: 20px;
    color: #26a2ff;
  }

  .mint-popup-4 {
    width: 100%;
    height: 275px;
  }

  .mint-popup-4 h1{
    backface-visibility: hidden;
  }

  .control {
    height: 35px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 0 10px 0 10px;
  }
</style>
