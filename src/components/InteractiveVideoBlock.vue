<template>
  <div class="InteractiveVideoBlock">
    <div class="cw-iav-wrapper">
      <video class="cw-iav-player" id="video" oncontextmenu="return false;">
          <source :src="videoSrc" type="video/mp4">
      </video>
      <div class="cw-iav-overlay-wrapper">
          <div class="cw-iav-overlay-content cw-iav-overlay-content-default" >
              <h2 class="cw-iav-overlay-content-title"></h2>
              <p class="cw-iav-overlay-content-text"></p>
          </div>
      </div>
      <div class="cw-iav-stop-wrapper">
          <div class="cw-iav-stop-content cw-iav-stop-content-default" >
              <h2 class="cw-iav-stop-content-title"></h2>
              <p class="cw-iav-stop-content-text"></p>
              <button class="button cw-iav-stop-button-continue">weiter</button>
          </div>
      </div>
    </div>
    <div class="cw-iav-controls">
      <div class="cw-iav-range" ></div>
      <span class="cw-iav-time"></span>
      <button class="cw-iav-playbutton" name="play"></button> 
      <button class="cw-iav-stopbutton" name="stop"></button>  
    </div>
  </div>
</template>

<script>
export default {
    name: 'InteractiveVideoBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data() {
      return{
        overlays: [],
        stops: [],
        videoSrc: ''
      }
    },
    beforeMount() {
      this.overlays = this.block.data.iav_overlays;
      this.stops = this.block.data.iav_stops;
      if(this.block.data.iav_source.external) {
        this.videoSrc = this.block.data.iav_source.url;
      } else {
          this.videoSrc = this.coursewarePath + './' + this.block.data.iav_source.file_id + '/' + this.block.data.iav_source.file_name;
      }
    },
    methods: {
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
              font-size: 1.5em;
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
}
</style>
