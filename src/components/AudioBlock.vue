<template>
    <div class="AudioBlock">
        <audio 
            class="cw-audio-player"
            ref="audio"
            @timeupdate='onTimeUpdateListener'
            @loadeddata="setDuration"
        >
            <source :src="path" type="audio/mpeg" />
        </audio>
        <h1 class="block-header">{{ block.data.audio_description}}</h1>
        <div class="cw-audio-container">
            <div class="cw-audio-controls">
                <input 
                    class="cw-audio-range"
                    ref="range"
                    type="range"
                    :value="currentSeconds"
                    min="0"
                    :max="Math.round(durationSeconds)"
                    @input="rangeAction"
                >
                <span class="cw-audio-time">{{currentTime}} / {{durationTime}}</span>
                <button name="stop" class="cw-audio-stopbutton" @click="stopAudio"></button>
                <button name="play" class="cw-audio-playbutton" @click="playAudio"></button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AudioBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data() {
        return {
            currentSeconds: 0,
            durationSeconds: 0
        }
    },
    computed: {
        currentTime() {
            return this.seconds2time(this.currentSeconds);
        },
        durationTime() {
            return this.seconds2time(this.durationSeconds);
        },
        path() {
            if (this.block.data.audio_source == 'cw') {
                return this.coursewarePath + this.block.data.audio_path;
            } else {
                return this.block.data.audio_path;
            }
        }
    },
    methods:{
        rangeAction() {
            if (this.$refs.range.value != this.currentSeconds) {
                this.$refs.audio.currentTime = this.$refs.range.value;
            }
        },
        setDuration() {
            this.durationSeconds = this.$refs.audio.duration;
        },
        playAudio(){
            this.$refs.audio.play();
        },
        stopAudio() {
            this.$refs.audio.pause();
            this.$refs.audio.currentTime =  0;
        },
        onTimeUpdateListener() {
            this.currentSeconds = this.$refs.audio.currentTime;
        },
        seconds2time(seconds) {
            seconds = Math.round(seconds);
            let hours   = Math.floor(seconds / 3600),
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

.cw-audio-container {
    border: solid thin #ccc;
    padding-top: 2em;
}

.cw-audio-controls {
    text-align: right;
    padding: 0 0.5em;
}

.cw-audio-range {
    margin: 0 5px 10px 0;
    &::-moz-focus-outer {
        border: 0;
    }
    &.ui-widget-content {
        background-color: #24437C;
    }

    .ui-widget-header {
        background-color: #fdfdfd;
    }

    .ui-slider-handle {
        border-radius: 20px;
        width: 1em;
        height: 1.7em;
        top: -0.5em;
        background-color: #ddd;
        border-color: #ccc;
        cursor: pointer;
        margin-left: -2px;
    }
}

.cw-audio-playbutton, .cw-audio-stopbutton {
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
    background-position: 6px center;
    display: inline-block;
}

.cw-audio-playbutton {
    background-image: url("../assets/icons/blue/play.svg");
    background-position: center center;
    background-repeat: no-repeat;
    background-size: 28px;

    &:hover {
        background-color: #24437C;
        background-image: url("../assets/icons/white/play.svg");
    }
}

.cw-audio-stopbutton {
    background-image: url("../assets/icons/blue/stop.svg");
    background-position: center center;
    background-repeat: no-repeat;
    background-size: 24px;

    &:hover {
        background-color: #24437C;
        background-image: url("../assets/icons/white/stop.svg");
    }
}


.cw-audio-playbutton-playing {
    background-image: url("../assets/icons/blue/pause.svg");
    background-position: center center;
    background-repeat: no-repeat;
    background-size: 24px;

    &:hover {
        background-image: url("../assets/icons/white/pause.svg");
    }
}

.cw-audio-time {
    position: relative;
    top: -1em;
    color: #333;
}

.cw-audio-range { 
    display: block;
    margin: 0 auto 1.5em;
    -webkit-appearance: none;
    position: relative;
    overflow: hidden;
    height: 18px;
    width: 100%;
    cursor: pointer;
    border-radius: 0;
}

.cw-audio-range::-webkit-slider-runnable-track {
    background: #ddd;
}

.cw-audio-range::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 9px; /* 1 */
    height: 18px;
    background: #fff;
    box-shadow: -100vw 0 0 100vw #28497c;
    border: solid thin #888;
}

.cw-audio-range::-moz-range-track {
    height: 18px;
    background: #eee;
}

.cw-audio-range::-moz-range-thumb {
    background: #fff;
    height: 18px;
    width: 9px;
    border: solid thin #888;
    border-radius: 0 !important;
    box-shadow: -100vw 0 0 100vw #28497c;
    box-sizing: border-box;
}

.cw-audio-range::-ms-fill-lower { 
    background: #28497c;
}

.cw-audio-range::-ms-thumb { 
    background: #fff;
    border: solid thin #888;
    height: 18px;
    width: 9px;
    box-sizing: border-box;
}

.cw-audio-range::-ms-ticks-after { 
    display: none; 
}

.cw-audio-range::-ms-ticks-before { 
    display: none; 
}

.cw-audio-range::-ms-track { 
    background: #ddd;
    color: transparent;
    height: 18px;
    border: none;
}

.cw-audio-range::-ms-tooltip { 
    display: none;
}

</style>