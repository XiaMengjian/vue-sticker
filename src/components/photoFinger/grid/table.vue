<template>
    <div class="box">
      <ul v-for="(liData, rowindex) in ulData">
        <li v-for="(item, index) in liData" @click="liClick(rowindex, index)">
          <div v-if="item.imgUrl.length != 0" :key="index" :class="[ 'radiusImg' , { imgBoard : item.select }]"  @click="chooseImg($event, index, item.imgUrl)" style=" border-radius: 100%; background: rgb(231,238,246);">
          <img   v-lazy="item.imgUrl">
          </div>
          <div v-else="" :class="['radiusImg']">
            <span ></span>
          </div>
        </li>
      </ul>
    </div>
</template>

<script>
  export default {
    props: ['imgSrcs', 'isSwipeFinish'],
    data () {
      return {
        ulData: [],
        currentSelectIndex: -1,
        defaultImg: {imgUrl: '', select: false},
        tableImgSrc: this.imgSrcs
      };
    },
    created: function () {
      this.mixinData();
    },
    mounted: function () {

    },
    methods: {
      chooseImg: function (e, index, url) {
        this.currentSelectIndex = index;
        this.$emit('selectIndex', index, url);
      },
      liClick: function (rowIndex, index) {
        console.log('-------liClick--------' + rowIndex + '   index   '+ index);
        console.log(this.ulData[rowIndex][index]);
        this.reductionData();
        this.ulData[rowIndex][index].select = !this.ulData[rowIndex][index].select;
      },
      reductionData: function () {
        this.ulData.forEach(function (value) {
          value.forEach(function (value) {
            value.select = false;
          });
        });
      },
      mixinData: function () {
        this.ulData.length = 0;
        let imgs = new Array(12).fill(this.defaultImg);
        imgs = this.imgSrcs.concat(imgs).slice(0, 12);
        for (let i = 0; i < imgs.length; i += 4) {
          this.ulData.push(imgs.slice(i, i + 4));
        }
      }
    },
    watch: {
      isSwipeFinish: function () {
        console.log('isSwipeFinish');
        this.reductionData();
      },
      imgSrcs: function (val, oldVal) {
        this.mixinData();
      }
    },
    components: {

    }
  };
</script>

<style scoped lang="less" rel="stylesheet/less">
  .box{
    width: 100%;
    height: 100%;
    background: #f5f5f5;
    margin: 0 auto;
    color: #fff;
    display: flex;
    flex-direction: column;
  }
  ul{
    display: flex;
    justify-content: center;
    margin: 5px 10px 5px 10px;
    height: 25%;
  }

  li{
    display: table-cell;
    width: 25%;
    margin: 6px;
  }

  img, span{
    width: auto;
    height: auto;
    max-width: 100%;
    max-height: 100%;
  }

  .imgBoard {
    border: 1px solid #ff0000;
  }

  div {
    height: 100%;
  }

  .radiusImg {
    text-align: center;
    width: 67px;
    height: 67px;
    box-sizing: border-box;

    /*width: 100%;*/
    /*height: 0px;*/
    /*padding-bottom: 100%;*/
    /*overflow: hidden;*/
    /*margin: 0;*/
    /*position: relative;*/
  }

</style>
