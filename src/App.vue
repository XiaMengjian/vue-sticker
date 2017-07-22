<template>
  <div id="app" class="page-wrapper">
    <ul v-clickoutside="close">
      <li v-for="(url, index) in fingerDatas">
        <Finger :imgSrc="url" :index="index" @deleteElemet="parentDelete"></Finger>
      </li>
    </ul>
    <div style="margin: 0 auto" @click="pop">
      <h1>弹框</h1>
    </div>
    <Pop v-model="popupVisible" position="bottom"  class="mint-popup-4"  @input="currentValue" :closeOnClickModal="true">
      <div class="control">
        <p @click="cancelImg">x</p>
        <p>选择贴纸</p>
        <p @click="addImg">√</p>
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
          [ {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/1.png', isSelect: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/2.png', isSelect: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/3.png', isSelect: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/4.png', isSelect: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/5.png', isSelect: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/6.png', isSelect: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/8.png', isSelect: false},
            {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/9.png', isSelect: false}
          ],
          [ {imgUrl: 'http://res.af.raiyee.com/playcontrol/images/stickerBag/7.png', isSelect: false}
          ]
        ],
        fingerDatas: [],
        isShown: true,
        isSwiped: false
      };
    },
    methods: {
      pop: function () {
        this.popupVisible = !this.popupVisible;
      },
      close: function () {
        console.log('closeOutSide');
//        alert('clickOutSide');
      },
      currentValue: function (val) {
        this.swipedOrClose();
      },
      addImgTouchLi: function (index, url) {
        this.selectImgUrl = url;
      },
      addImg: function () {
        this.fingerDatas.push(this.selectImgUrl);
        this.popupVisible = false;
      },
      cancelImg: function () {
        this.popupVisible = false;
      },
      parentDelete: function (isShown) {
        this.isShown = false;
      },
      swipedOrClose: function () {
        this.isSwiped = !this.isSwiped;
      },
      tapActive: function (index) {
        alert(index);
      }
    },
    watch: {
      popupVisible: function () {
        this.selectImgUrl = '';
        this.swipedOrClose();
      },
      imgUrls: function () {

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
  p {
    margin-bottom: 10px;
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
