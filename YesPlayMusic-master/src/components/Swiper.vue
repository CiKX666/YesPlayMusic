<template>
  <!-- Swiper -->
  <div class="swiper-container" id="floor1Swiper" ref="mySwiper">
    <div class="swiper-wrapper">
      <div
        class="swiper-slide"
        v-for="carousel in BannerImg"
        :key="carousel.encodeId"
      >
        <img :src="carousel.imageUrl" />
      </div>
    </div>
    <!-- Add Pagination -->
    <div class="swiper-pagination"></div>
  </div>
</template>

<script>
//引入swiper
import Swiper from 'swiper';
import 'swiper/css/swiper.css';
import { banner } from '@/api/playlist';
export default {
  name: 'Swiper',
  data() {
    return {
      BannerImg: [],
    };
  },
  mounted() {
    banner().then(data => {
      this.BannerImg = data.banners;
    });
  },
  watch: {
    BannerImg: {
      //立即监听：不管你数据有没有变化，一上来就立即监听一次
      //为什么watch监听不到list：因为这个数据从来没有发生变化（数据是父亲给的，父亲给的时候就是一个对象，对象里面该有的数据都是有的）
      immediate: true,
      handler() {
        //只能监听到数据已经有了，但是v-for动态渲染结构我们还是没有办法确定渲染完成，因此还是需要nextTick
        this.$nextTick(() => {
          var mySwiper = new Swiper(this.$refs.mySwiper, {
            loop: true, // 循环模式选项
            autoplay: true,
            //  如果需要分屏
            pagination: {
              el: '.swiper-pagination',
              clickable: true,
            },
          });
        });
      },
    },
  },
};
</script>

<style scoped></style>
