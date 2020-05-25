<template>
  <div class="InteractiveVideoBlock">
    <div class="cw-iav-wrapper">
      <video class="cw-iav-player" id="video" oncontextmenu="return false;" ref="video">
          <source :src="videoSrc" type="video/mp4">
      </video>
      <div class="cw-iav-overlay-wrapper" v-show="showOverlay">
          <div class="cw-iav-overlay-content" :class="[activeOverlay.position, activeOverlay.type, activeOverlay.color]">
              <h2 class="cw-iav-overlay-content-title">{{activeOverlay.title}}</h2>
              <p class="cw-iav-overlay-content-text">{{activeOverlay.content}}</p>
          </div>
      </div>
      <div class="cw-iav-stop-wrapper" v-show="showStop">
          <div class="cw-iav-stop-content" :class="[activeStop.position, activeStop.type, activeStop.color]" >
              <h2 class="cw-iav-stop-content-title">{{activeStop.title}}</h2>
              <p class="cw-iav-stop-content-text">{{activeStop.content}}</p>
              <button class="button cw-iav-stop-button-continue" @click="solveStop">weiter</button>
          </div>
      </div>
    </div>
    <div 
        class="cw-iav-seekbar-wrap"
        ref="seekbar"
        @mousedown = 'grabSeekbar'
        @touchstart = 'grabSeekbar'
        @touchmove = 'moveSeekbar'
        @touchend = 'releaseSeekbar'
      >
        <div class="cw-iav-seekbar-current" :style = '{ transform: "scaleX(" + getProgressRate + ")" }' ></div>
        <div class="cw-iav-seekbar-back"></div>
    </div>
    <div class="cw-iav-controls">
      <span class="cw-iav-time">{{getCurrentTime}}/{{getDuration}}</span>
      <button class="cw-iav-playbutton" :class="{playing:isPlaying}" name="play" @click="playVideo"></button> 
      <button class="cw-iav-stopbutton" name="stop" @click="stopVideo"></button>  
    </div>
  </div>
</template>

<script>
const debounce = (callback, duration) => {
  var timer;
  return function(event) {
    clearTimeout(timer);
    timer = setTimeout(function(){
      callback(event);
    }, duration);
  };
}
export default {
    name: 'InteractiveVideoBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data() {
      return{
        overlays: [],
        activeOverlay: Object,
        showOverlay: false,
        stops: [],
        activeStop: Object,
        showStop: false,
        videoSrc: '',
        media: null,
        seekbar: null,
        seekbarWidth: 0,
        seekbarOffsetX: 0,
        time: 0,
        duration: 0,
        isPlaying: false,
        isGrabbingSeekbar: false,
      }
    },
    beforeMount() {
      this.overlays = this.block.data.iav_overlays;
      this.stops = this.block.data.iav_stops;
      this.stops.forEach(stop => {
          stop.done = false;
      });
      if(this.block.data.iav_source.external) {
        this.videoSrc = this.block.data.iav_source.url;
      } else {
          this.videoSrc = this.coursewarePath + './' + this.block.data.iav_source.file_id + '/' + this.block.data.iav_source.file_name;
      }
    },
    mounted: function() {
      // init
      this.media = this.$refs.video;
      this.seekbar = this.$refs.seekbar;
      this.reLayoutSeekbar();

      // addEventListener
      window.addEventListener('resize', debounce(() => {
        this.reLayoutSeekbar();
      }), 100);
      document.addEventListener('mousemove', (event) => {
        this.moveSeekbar(event);
      });
      document.addEventListener('mouseup', (event) => {
        this.releaseSeekbar(event);
      });
      this.media.addEventListener('loadedmetadata', () => {
        this.duration = this.media.duration;
      });
      this.media.addEventListener('ended', () => {
        this.media.currentTime = 0;
        this.isPlaying = false;
      });
    },
    computed: {
      getProgressRate: function() {
        return this.time / this.duration;
      },
      getCurrentTime: function() {
        return this.convertSecondsToTime(this.time);
      },
      getDuration: function() {
        return this.convertSecondsToTime(this.duration);
      },
    },
    watch: {
        time: function(val) {
            let view = this;
            let setOverlay = false;

            this.overlays.forEach(overlay => {
                if((overlay.start < val) && (overlay.end > val)) {
                    this.activeOverlay = overlay;
                    setOverlay = true;
                }
            });
            if (setOverlay && !this.showOverlay) {
                this.showOverlay = true;
            } 
            if (!setOverlay){
                this.showOverlay = false;
            }

            let setStop = false;

            this.stops.every(stop => {
                if(stop.moment <= Math.round(val) && (!stop.done)) {
                    this.activeStop = stop;
                    setStop = true;
                    return false;
                } else {
                    return true;
                }
            });
            if (setStop) {
                this.showStop = true;
                this.media.pause();
                this.media.currentTime = this.activeStop.moment;
                this.time = this.activeStop.moment;
                this.reLayoutSeekbar();
            }
            if (!setStop){
                this.showStop = false;
            }
        }
    },
    methods: {
        playVideo() {
            if (this.media.paused) {
                this.media.play();
                this.isPlaying = true;
                this.loop();
            } else {
                this.media.pause();
                this.isPlaying = false;
            }
        },
        stopVideo() {
            this.media.pause();
            this.media.currentTime = 0;
            this.time = 0;
            this.isPlaying = false;
            this.stops.forEach(stop => {
                stop.done = false;
            });
        },
        loop: function() {
            this.time = this.media.currentTime;
            if (!this.isPlaying) return;
            requestAnimationFrame(() => {
            this.loop();
            });
        },
        solveStop() {
            let view = this;
            this.stops.every(stop => {
                if (stop.id == view.activeStop.id) {
                    stop.done = true;
                    return false;
                } else {
                    return true;
                }
            });
            this.showStop = false;
            this.media.play();
        },
        grabSeekbar: function(event) {
            event.preventDefault();
            this.isGrabbingSeekbar = true;
            this.time = this.media.currentTime = event.layerX / this.seekbarWidth * this.duration;
            this.media.pause();
        },
        moveSeekbar: function(event) {
            event.preventDefault();
            if (!this.isGrabbingSeekbar) return;
            this.time = this.media.currentTime = (event.clientX - this.seekbarOffsetX - window.pageXOffset) / this.seekbarWidth * this.duration;
        },
        releaseSeekbar: function(event) {
            event.preventDefault();
            this.isGrabbingSeekbar = false;
            if (this.isPlaying) {
            this.media.play();
            }
        },
        reLayoutSeekbar: function() {
            this.seekbarWidth = this.seekbar.clientWidth;
            this.seekbarOffsetX = this.seekbar.getBoundingClientRect().left;
        },
        convertSecondsToTime: function(time) {
            let seconds = Math.floor(time % 60);
            if (seconds < 10) seconds = '0' + seconds;
            let minutes = Math.floor(time / 60 % 60);
            return `${minutes}:${seconds}`
        },
        seconds2time(seconds) {
          var hours   = Math.floor(seconds / 3600),
              minutes = Math.floor((seconds - (hours * 3600)) / 60),
              time = '';

          seconds = seconds - (hours * 3600) - (minutes * 60);

          if (hours != 0) {
            time = hours + ':';
          }
          if (minutes != 0 || time !== '') {
            minutes = (minutes < 10 && time !== '') ? '0' + minutes : String(minutes);
            time += minutes + ':';
          }
          if (time === '') {
            time = (seconds < 10) ? '0:0' + seconds : '0:' + seconds;
          }
          else {
            time += (seconds < 10) ? '0' + seconds : String(seconds);
          }

          return time;
      }
    }
}
</script>

<style lang="less">
.InteractiveVideoBlock {
  .cw-iav-wrapper {
      width: 100%;
      position: relative;
  }

  .cw-iav-player,
  .cw-iav-overlay-wrapper,
  .cw-iav-stop-wrapper {
      width: 100%;
      height: auto;
      top: 0;
      left: 0;
  }

  .cw-iav-player{
      display: block;
      color: #fff; 
      z-index: -10;
  }

  .cw-iav-overlay-wrapper,
  .cw-iav-stop-wrapper {
      background-color: transparent;
      z-index: 10;
  }

  .cw-iav-overlay-edit-wrapper,
  .cw-iav-stop-edit-wrapper,
  .cw-iav-test-edit-wrapper {
      display: inline-block;
      margin-top: 1em;
  }

  .cw-iav-stop-button, 
  .cw-iav-stop-button-continue {
      position: absolute;
      right: 0;
      margin-right: 1em;
      bottom: 0;
      margin-bottom: 1em;
      
  }

  .cw-iav-stop-content,
  .cw-iav-overlay-content {
      position: absolute;
      overflow: auto;
      
      &.cw-iav-black-white {
          background-color: rgba(255,255,255,0.9);
          border-color: #fff;
          h2, p { color: #000; border-color: #000;}
      }
      &.cw-iav-white-black {
          background-color: rgba(0,0,0,0.7);
          border-color: #000;
          h2, p { color: #fff; border-color: #fff;}
      }
      &.cw-iav-red-white {
          background-color: rgba(255,255,255,1);
          border-color: #f00;
          h2, p { color: #f00; border-color: #f00;}
      }
      &.cw-iav-blue-white {
          background-color: rgba(255,255,255,0.9);
          border-color: #28497c;
          h2, p { color: #28497c; border-color: #28497c;}
      }

      &.cw-iav-box {
          width: 40%;
          height: 40%;
          padding: 1em 2em;
          border-width: thick;
          border-style: solid;
          text-align: center;

          &.cw-iav-h-left {
              left: 1%;
          }
          &.cw-iav-h-center {
              left: 25%;
          }
          &.cw-iav-h-right {
              right: 1%;
          }
          &.cw-iav-v-top {
              top: 2%;
          }
          &.cw-iav-v-center {
              top: 25%;
          }
          &.cw-iav-v-bottom {
              top: 50%;
          }

          h2 {
              border-bottom-width: 2px;
              border-bottom-style: solid;
              font-size: 1.5em;
              margin-top: 0.5em;
          }

          p {
              text-align: justify;
          }
      }

      &.cw-iav-wide {
          height: 15%;
          padding: 0.5em 1em;
          border-top-width: thick;
          border-top-style: solid;
          border-bottom-width: thick;
          border-bottom-style: solid;
          text-align: left;
          padding: 1em 2em;
          width: 85%;

          &.cw-iav-h-left {
              left: 0;
          }
          &.cw-iav-h-center {
              left: 4%;
          }
          &.cw-iav-h-right {
              right: 0;
          }
          &.cw-iav-v-top{
              top: 5%;
          }
          &.cw-iav-v-center {
              top: 40%;
          }
          &.cw-iav-v-bottom {
              top: 75%;
          }

          h2 {
              font-size: 2em;
              border-bottom: none;
              margin: 0;
              padding: 0;
          }

          p {
              font-size: 1.25em;
          }
      }

      &.cw-iav-high {
          width: 40%;
          height: 100%;
          border: none;
          text-align: left;
          padding: 1em 1.5em;

          &.cw-iav-h-left {
              left: 0;
          }
          &.cw-iav-h-center {
              left: 28.5%;
          }
          &.cw-iav-h-right {
              right: 0;
          }
          &.cw-iav-v-top,
          &.cw-iav-v-center,
          &.cw-iav-v-bottom
          {
              top: 0;
          }

          h2 {
              font-size: 2em;
              border-bottom: none;
              margin: 0;
              padding: 0;
          }

          p {
              font-size: 1.1em;
              text-align: justify;
          }
      }
      
      &.cw-iav-full {
          width: 90%;
          height: 90%;
          padding: 1em 2em;
          border-width: thick;
          border-style: solid;
          text-align: center;
          top: 1%;
          left: 1%;

          h2 {
              border-bottom-width: 2px;
              border-bottom-style: solid;
              font-size: 1.5em;
              margin-top: 0.5em;
          }

          p {
              text-align: justify;
          }
      }
  }

  .cw-iav-overlay-logo {
      color: rgba(255,255,255,0.4); 
      text-align: left;
      font-size: 20px;
      background-color: transparent;
      position: absolute;
      width: 2em;
      height: 20px;
      top: 0;
      left: 0;
  }

  .cw-iav-controls {
      width: 100%;
      margin-top: -5px;
      text-align: right;

      div.cw-iav-range {
          margin: 24px 0;

          div.ui-widget-header {
              background-color: #28497c;
          }
          .ui-slider-horizontal.ui-slider-handle {
              top: -6px;
              margin-left: -0.6em;
              border-radius: 12px;
              height: 24px;
              width: 24px;
          }
      }

      span.cw-iav-time {
          position: relative;
          top: -1em;
          color: #333;
      }

      .cw-iav-playbutton, .cw-iav-stopbutton {
          color: #28497c;
          border: solid thin #28497c;
          background-color: #fff;
          min-height: 27px;
          line-height: 130%;
          padding: 5px 15px 5px 30px;
          cursor: pointer;
          font-size: 14px;
          box-sizing: border-box;
          text-align: center;
          text-decoration: none;
          vertical-align: bottom;
          white-space: nowrap;
          min-width: unset;
          margin: 5px;
          height: 46px;
          width: 46px;
          display: inline-block;
          &:hover {
            outline: 0;
            color: #fff;
            background-color: #28497c;
          }
      }

      .cw-iav-playbutton {
          background-image: url("../assets/icons/blue/play.svg");
          background-position: center center;
          background-repeat: no-repeat;
          background-size: 28px;
          &:hover {
            background-image: url("../assets/icons/white/play.svg");
          }
          &.playing {
            background-image: url("../assets/icons/blue/pause.svg");
              &:hover {
                background-image: url("../assets/icons/white/pause.svg");
              }
          }
      }

      .cw-iav-stopbutton {
          background-image: url("../assets/icons/blue/stop.svg");
          background-position: center center;
          background-repeat: no-repeat;
          background-size: 24px;
          &:hover{
            background-image: url("../assets/icons/white/stop.svg");
          }
      }

      .cw-iav-playbutton-playing {
        background-image: url("../assets/icons/blue/pause.svg");
          background-position: center center;
          background-repeat: no-repeat;
          background-size: 24px;
          &:hover {
            background-image: url("../assets/icons/white/pause.svg");
          }
      }
  }
  .cw-iav-seekbar-wrap {
      cursor: pointer;
      position: relative;
      margin-bottom: 10px;
      padding: 10px 0;

    .cw-iav-seekbar-current, .cw-iav-seekbar-back {
      height: 3px;
      position: absolute;
      top: 10px; right: 0; left: 0;
    }
    .cw-iav-seekbar-current {
      z-index: 2;
      background-color: #28497c;
      transform: scaleX(0);
      transform-origin: left;
    }
    .cw-iav-seekbar-back {
      background-color: #ddd;
    }
  }
}
</style>
