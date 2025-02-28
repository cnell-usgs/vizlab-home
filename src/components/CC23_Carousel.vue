<template v-slot:figures>
  <div class="maxWidth carouselContainer">
    <carousel
      class="image-slider"
      :autoplay="false"
      :autoplay-hover-pause="true"
      :per-page="4"
      navigation-enabled
      :speed="800"
    >
      <slide
        v-for="chart in charts"
        :id="`chart-challenge-${chart.id}`"
        :key="chart.id"
        class="slide"
      >
        <div class="slider-image-container">
          <picture>
            <source
              :srcset="getImgUrl(chart.image_basename, 'webp')"
              type="image/webp"
            >
            <source
              :srcset="getImgUrl(chart.image_basename, chart.image_type)"
              type="image/jpg"
            >
            <img 
              :id="`chart-challenge-${chart.id}`"
              v-img="{
                thumbnails: true,
                group: 'chart-challenge', 
                title: chart.caption
              }"
              class="sliderImage"
              :src="getImgUrl(chart.image_basename, chart.image_type)"
              :alt="chart.image_alt"
              loading="lazy"
            >
          </picture>
        </div>
      </slide>
    </carousel>
  </div>
</template>

<script>
    import { Carousel, Slide } from 'vue-carousel';
    import CC23 from "@/assets/content/CC23.js";
    export default {
        name: 'ChartChallenge23Carousel',
        components:{
            Carousel,
            Slide
        },
        data() {
          return {
            charts: CC23.chartChallengeCharts
          }
        },
        mounted(){
          // sort charts
          this.charts.sort((a,b) => new Date(a.date) - new Date(b.date))
          // for each chart, build caption for use w/ v-img
          this.charts.forEach(chart => {
            chart.caption = 'Contribution for ' + chart.cc_prompt + ' by ' + chart.author + '. Original available <a href=' + chart.drupal_url + ' target="_blank">here</a>. Released on <a href=' + chart.tweet_url + ' target="_blank">Twitter</a>.'
          })
        },
        methods: {
          getImgUrl(pic, extension) {
            // TODO: alternative if image is not given or broken
            return 'https://labs.waterdata.usgs.gov/visualizations/charts/' + pic + '.' + extension
          }
        }
    }
</script>

<style scoped lang="scss">
  .carouselContainer {
    max-width: 98%;
  }
  .image-slider {
    margin: auto;
    max-width: 70rem;
    *:focus{
      outline: none;
    }
  }
  .slide {
    margin: 0;
    padding: 0;
    select:focus{
      outline: none;
    }
  }
  .slide:hover {
    transform: translate3D(0,-0.5px,0) scale(1.05);
    transition: all .3s ease; 
  }
  .slider-image-container {
    padding: 10px;
    display: grid;
    grid-template-columns: max-content;
    height: 200px;
    max-width: 275px;
    align-content: center;
    justify-content: center;
    img {
      max-width: 255px;
      max-height: 180px;
    }
  }
  </style>
  <style lang="scss">
  .VueCarousel-dot-container {
    margin-top: 0px !important;
  }
  .VueCarousel-dot {
    margin-top: 0px !important;
  }
  .VueCarousel-navigation {
    @media (max-width: 600px) {
      display: none;
    }
  }
  //Hides v-img title element
  .title-v-img{
    display: none;
  }
  .fullscreen-v-img{
    position: relative;
  }
  .footer-v-img {
    justify-content: start !important;
  }
  .footer-v-img img {
    width: auto !important;
    height: 40px !important;
  }
  #captionArea{
    background: rgba(255,255,255,1);
    position: absolute;
    width: 100%;
    bottom: 0px;
    z-index: 9000;
    text-align: left;
    padding: 20px 10px;
    color: black;
    font-size: 1em;
    display: flex;
    justify-content: center;
    @media screen and (max-width: 600px) {
      padding: 20px 10px;
      font-size: .8em;
    }
    .caption{
      margin: 0 auto 70px auto;
      max-width: 900px;
      line-height: 1.5em;
    }
  }
</style>