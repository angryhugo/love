<template>
  <div @touchstart.once="playAudio">
    <div class="pu">
      我最亲爱的<small v-cloak>{{days}}</small>
      <img src="../assets/img/favicon.png" alt="" width="36">
    </div>
    <!-- Swiper -->
    <div class="swiper-container">
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
    <audio id="my-audio" src="http://owntjivne.bkt.clouddn.com/little-luck.mp3" autoplay="autoplay"></audio>
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
        dateArr: dateArr
      }
    },
    mounted() {
      swiperStart = document.querySelector(".icon-start");
      swiperPause = document.querySelector(".icon-pause");
      this.swiper();
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
</style>
