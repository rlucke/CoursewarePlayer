<template>
    <div class="VideoBlock">
        <h1 class="block-header">
            {{block.data.videoTitle}}
        </h1>
        <div v-if="webvideos.length > 0" class="video-wrapper" :class="[block.data.aspect]">
            <video width="100%" controls>
                <source v-for="webvideo in webvideos" :key="webvideo.file_id" :src="webvideo.url" :type="('video/'+ webvideo.type)" :media="webvideo.media" >
            </video>
        </div>
        <div v-else class="video-wrapper" :class="[block.data.aspect]">
            <iframe :src="block.data.url" scrolling="no" allowfullscreen />
        </div>
    </div>
</template>

<script>
export default {
    name: 'VideoBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    computed: {
        webvideos() {
            let view = this;
            let webvideos = [];
            if (this.block.data.webvideo == '') {return webvideos}
            this.block.data.webvideo.forEach(video => {
                let webvideo = {};
                if (video.source == 'file') {
                    webvideo.url = view.coursewarePath + './' + video.file_id + '/' + video.file_name;
                } else {
                    webvideo.url = video.src;
                }
                webvideo.type = video.type;
                webvideo.filename = video.file_name;
                webvideo.file_id = video.file_id;
                webvideo.media = video.media;
                webvideos.push(webvideo);
            });

            return webvideos;
        }
    }
}
</script>

<style lang="less">
.VideoBlock {
    .video-wrapper {
        background-color: #ccc;

        &.aspect-169 {
            padding-bottom: 56.25%; /* 16:9 ratio; 9/16*100 */
        }

        &.aspect-43 {
            padding-bottom: 75%; /* 4:3 ratio; 3/4*100 */
        }

        position: relative;
        padding-top: 0px;
        height: 0;
        overflow: hidden;
        iframe {
            border: none;
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
        video {
            width: 100%;
        }
    }
}
</style>
