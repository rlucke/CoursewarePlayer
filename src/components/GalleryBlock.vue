<template>
  <div class="GalleryBlock" :style="{height: block.data.gallery_height+'px'}">
    <div v-for="(image, i) in images" :key="i" ref="images" class="cwGallerySlides cwGalleryFade" >
      <div class="cwGalleryNumberText">
        {{i+1}} / {{images.length}}
      </div>
      <img :src="coursewarePath + image" :style="{height: block.data.gallery_height+'px'}" />
      <div v-if="block.data.gallery_show_names == '1'" class="cwGalleryFileName">
        <span>{{image.split('/')[2]}}</span>
      </div>
    </div>
    <div v-if="block.data.gallery_hidenav == '0'" class="cwGalleryNav">
      <a class="prev" @click="plusSlides(-1)">&#10094;</a>
      <a class="next" @click="plusSlides(1)">&#10095;</a>
    </div>
  </div>
</template>

<script>
export default {
    name: 'GalleryBlock',
    props:{
      block: Object,
        coursewarePath: String
    },
    data () {
      return {
          images: [],
          slideIndex: 0
      }
    },
    mounted(){
      this.images = this.block.data.gallery_file_paths;
      this.$nextTick(() => { 
        this.showSlides(0);
        if (this.block.data.gallery_autoplay == '1') {
          this.slideIndex = -1;
          this.playSlides();
        }
      });
    },
    methods: {
      plusSlides: function(n) {
          this.showSlides(this.slideIndex += n);
      },
      showSlides: function(n) {
          let slides = this.$refs.images;
          if (n > slides.length-1) {this.slideIndex = 0}
          if (n < 0) {this.slideIndex = slides.length-1}
          slides.forEach(slide => {
            slide.style.display = 'none';
          });
          slides[this.slideIndex].style.display = "block";
      },
      playSlides: function() {
        let slides = this.$refs.images;
        slides.forEach(slide => {
          slide.style.display = 'none';
        });
        this.slideIndex++;
        if (this.slideIndex > slides.length-1) {this.slideIndex = 0}
        slides[this.slideIndex].style.display = "block";
        setTimeout(this.playSlides, this.block.data.gallery_autoplay_timer);
      }
    }
}
</script>

<style>
.GalleryBlock {
  max-width: 890px;
  position: relative;
  margin: auto;
}

.cwGallerySlides {
  display: none;
}

.cwGallerySlides > img {
    max-width: 890px;
    display: block;
    margin-left: auto;
    margin-right: auto;
}

.prev, .next {
  cursor: pointer;
  position: absolute;
  background-color: rgba(255,255,255,0.4);
  top: 50%;
  width: auto;
  margin-top: -22px;
  padding: 16px;
  color: #28497c;
  font-weight: bold;
  font-size: 18px;
  transition: 200ms ease;
  user-select: none;
}

.next {
  right: 0;
}

.prev:hover, .next:hover {
  color: #fff;
  background-color: #28497c;
}

.cwGalleryFileName {
  color: #fff;
  font-size: 15px;
  padding: 8px 12px;
  position: absolute;
  bottom: 8px;
  width: 100%;
  text-align: center;
}

.cwGalleryFileName > span {
  background-color: rgba(0,0,0,0.4);
  padding: 0.5em;
}

.cwGalleryNumberText {
  color: #fff;
  font-size: 12px;
  padding: 8px 12px;
  position: absolute;
  top: 0;
  background-color: rgba(0,0,0,0.4);
}

.cwGalleryFade {
  -webkit-animation-name: fade;
  -webkit-animation-duration: 1.5s;
  animation-name: fade;
  animation-duration: 1.5s;
}

@-webkit-keyframes fade {
  from {opacity: .4}
  to {opacity: 1}
}

@keyframes fade {
  from {opacity: .4}
  to {opacity: 1}
}
</style>