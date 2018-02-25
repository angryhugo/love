<template>
  <div class="content" @touchstart.once="playAudio">
    <div class="pu">
      <img src="../assets/img/favicon.png">
      我最亲爱的<small v-cloak>{{days}}</small>
    </div>
    <!-- Swiper -->
    <div class="swiper-container" v-show="isSwiperShown">
      <span class="iconfont icon-start" @click="startSwiper"></span>
      <span class="iconfont icon-pause" @click="stopSwiper"></span>
      <div class="swiper-wrapper">
        <div class="swiper-slide" v-for="dateDesc in dateArr" v-cloak>
          <p class="desc">{{dateDesc.desc}}<small v-if="dateDesc.tip">{{dateDesc.tip}}</small></p>
          <p class="date">{{dateDesc.date}}</p>
          <p class="distance">{{dateDesc.distance}}km</p>
          <img :src="getImgUrl(dateDesc.date)" class="swiper-lazy" width="95%">
        </div>
      </div>
    </div>
    <!--list-->
    <div class="photo-list clearfix" v-show="!isSwiperShown">
      <div class="photo-item" v-for="dateDesc in dateArr" @click="showPhoto(getImgUrl(dateDesc.date))" v-cloak>
        <p class="desc">{{dateDesc.desc}}<small v-if="dateDesc.tip">{{dateDesc.tip}}</small></p>
        <p class="date">{{dateDesc.date}}</p>
        <img :src="getImgUrl(dateDesc.date)">
      </div>
    </div>
    <div class="photo-popup" v-show="photoShown">
      <div class="mask"></div>
      <div class="photo-content">
        <span class="photo-close" @click="closePhoto"></span>
        <img :src="photoUrl" width="600" height="600">
      </div>
    </div>
    <audio id="my-audio" src="http://owntjivne.bkt.clouddn.com/almost-lover.mp3" autoplay="autoplay" loop="loop"></audio>
  </div>
</template>

<script>
  var Swiper = require('../assets/plugin/swiper/swiper.min');
  var dateArr = require("../assets/data/date").data;

  var swiper, swiperStart, swiperPause;
  var qiniuBaseUrl = "http://owntjivne.bkt.clouddn.com/";
  var dateTime = new Date().getTime();
  var startDayTime = new Date("2017", "7", "28").getTime();

  export default {
    name: 'Love',
    data() {
      return {
        days: parseInt((dateTime - startDayTime) / (24 * 60 * 60 * 1000))+"days",
        dateArr: dateArr,
        isSwiperShown: (document.body.clientWidth<1200),
        photoShown: false,
        photoUrl: ""
      }
    },
    mounted() {
      const self = this;
      swiperStart = document.querySelector(".icon-start");
      swiperPause = document.querySelector(".icon-pause");
      window.onresize = () => {
        return (() => {
          self.isSwiperShown = document.body.clientWidth<1200;
        })()
      };
      this.swiper();
    },
    watch: {
      isSwiperShown (val) {
        if (!this.timer) {
          this.isSwiperShown = val;
          this.timer = true;
          let self = this;
          if(this.isSwiperShown) {
            this.startSwiper();
          } else {
            this.stopSwiper();
          }
          setTimeout(function () {
            self.timer = false
          }, 400);
        }
      }
    },
    methods: {
      getImgUrl: function (date) {
        return qiniuBaseUrl + date + '.jpg?t=' + dateTime
      },
      swiper: function () {
        swiper = new Swiper('.swiper-container', {
          // Enable lazy loading
          // lazyLoading: true,
          autoplay: 3000,
          autoplayDisableOnInteraction: false,
          loop: true
        });
      },
      startSwiper: function () {
        swiper.startAutoplay();
        swiperStart.style.display="none";
        swiperPause.style.display="block";
      },
      stopSwiper: function () {
        swiper.stopAutoplay();
        swiperPause.style.display="none";
        swiperStart.style.display="block";
      },
      playAudio: function() {
        document.getElementById("my-audio").play();
      },
      showPhoto: function (url) {
        this.photoShown = true;
        this.photoUrl = url;
      },
      closePhoto: function () {
        this.photoShown = false;
//        this.photoUrl = "";
      }
    }
  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @import "http://at.alicdn.com/t/font_436089_3vdj16v6rbkgwrk9.css";
  @import "../assets/plugin/swiper/swiper.min.css";

  html, body {
    position: relative;
    height: 100%;
  }

  .pu {
    height: 40px;
    margin: 5% 2.5% 2.5%;
    padding-bottom: 5px;
    border-bottom: 1px solid #cdced3;
    line-height: 40px;
    font-size: 30px;
  }

  .pu img {
    border-radius: 50%;
    float: right;
    width: 36px;
    height: 36px;
  }

  .pu small {
    color: #e38;
    font-size: 18px;
    margin-left: 2%;
  }

  .swiper-container {
    width: 100%;
    height: auto;
  }

  .swiper-slide img {
    width: auto;
    height: auto;
    max-width: 95%;
    max-height: 95%;
    border-radius: 3%;
    margin-left: 2.5%;
  }

  .swiper-slide p {
    margin-left: 2.5%;
  }

  .swiper-slide p.distance {
    margin-bottom: 9px;
    height: 20px;
    line-height: 20px;
    color: #017bff;
    font-size: 14px;
  }

  .swiper-slide p.desc {
    height: 32px;
    line-height: 32px;
    color: #000;
    font-size: 20px;
  }

  .swiper-slide p.desc small {
    color: #e38;
    font-size: 16px;
    margin-left: 2%;
  }

  .swiper-slide p.date {
    height: 24px;
    line-height: 24px;
    color: #8e8e93;
    font-size: 16px;
  }

  .iconfont {
    font-size: 24px;
    line-height: 32px;
    position: absolute;
    right: 2.5%;
    z-index: 9;
    display: block;
    height: 32px;
    width: 24px;
  }

  .icon-start {
    display: none;
  }

  .photo-list {
    padding-top: 40px;
    /*display: flex;*/
    /*flex-wrap: wrap;*/
    margin-left: 200px;
  }

  .photo-item {
    float: left;
    position: relative;
    width: 300px;
    height: 300px;
    margin: 0 0 20px 20px;
    border-radius: 5px;
    overflow: hidden;
    cursor: zoom-in;
  }

  .photo-item img {
    width: 300px;
    height: 300px;
  }

  .photo-item p.date {
    position: absolute;
    background: #29e;
    color: #fff;
    padding: 0 6px;
    border-radius: 0 0 5px 0;
  }

  .photo-item p.desc {
    background: rgba(0,0,0,0.6);
    color: #fff;
    position: absolute;
    bottom: 0;
    padding: 0 2.5%;
    width: 95%;
    height: 32px;
    line-height: 32px;
    font-size: 18px;
  }

  .photo-item p.desc small{
    color: #e38;
    font-size: 16px;
    float: right;
  }

  .photo-popup {
    position: fixed;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
  }

  .mask {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    background-color: rgba(0,0,0,.8);
  }

  .photo-content {
    width: 600px;
    height: 600px;
    border: 2px solid #fff;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-top: -301px;
    margin-left: -301px;
    background-color: #fff;
  }

  .photo-close {
    position: absolute;
    width: 40px;
    height: 40px;
    right: -20px;
    top: -20px;
    border-radius: 50%;
    cursor: pointer;
    background-color: #fff;
    background-image: url(../assets/img/close.svg);
    transform: scale(0.75);
  }

  /*动画会引起lazyloading bug*/
  .swiper-slide:after {
    content: "";
    display: block;
    width: 50px;
    height: 50px;
    -webkit-animation: ball-scale infinite linear .75s;
    animation: ball-scale infinite linear .75s;
    border-radius: 100%;
    background-color: #ee3388;
    position: absolute;
    left: 50%;
    top: 50%;
    margin: -25px 0 0 -25px;
    z-index: -1;
  }

  @-webkit-keyframes ball-scale {
    0% {
      -webkit-transform: scale(.1);
      -ms-transform: scale(.1);
      -o-transform: scale(.1);
      transform: scale(.1);
      opacity: 1;
    }
    100% {
      -webkit-transform: scale(1);
      -ms-transform: scale(1);
      -o-transform: scale(1);
      transform: scale(1);
      opacity: 0;
    }
  }


  @keyframes ball-scale {
    0% {
      -webkit-transform: scale(.1);
      -ms-transform: scale(.1);
      -o-transform: scale(.1);
      transform: scale(.1);
      opacity: 1;
    }
    100% {
      -webkit-transform: scale(1);
      -ms-transform: scale(1);
      -o-transform: scale(1);
      transform: scale(1);
      opacity: 0;
    }
  }

  [v-cloak] {
    display: none;
  }

  @media screen and (min-width:1200px){
    .content {
      width: 1200px;
      margin: 0 auto;
    }
    .pu {
      position: fixed;
      width: 200px;
      margin: 40px 0 0 0;
      text-align: center;
      border: none;
      height: 200px;
      padding-bottom: 10px;
    }
    .pu img {
      display: block;
      width: 114px;
      height: 114px;
      float: none;
      margin: 0 auto 10px;
      border: 1px solid #b06e5d;
      padding: 5px;
    }
    .pu small {
      display: block;
      margin-left: 0;
    }
  }

</style>
