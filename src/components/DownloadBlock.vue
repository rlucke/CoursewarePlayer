<template>
    <div class="DownloadBlock">
        <h1 class="block-header">{{block.data.download_title}}</h1>
        <div class="cw-download-file">
            <div class="cw-download-info" :class="{['cw-fileicon-'+block.data.icon]:true}" > &nbsp; {{block.data.file}} </div>
                <div class="cw-download-button-wrapper">
                    <a :href="coursewarePath + block.data.href" :download="block.data.file" target="_blank"> 
                        <button name="download" class="cw-download-button">Herunterladen</button>
                    </a> 
                </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'DownloadBlock',
    props:{
        block: Object,
        coursewarePath: String
    }
}
</script>

<style lang="less">
.DownloadBlock{
    .cw-download-file{
        border: solid thin #ccc;
        padding: 1em;
    }

    .cw-download-info { 
        background-image: url("../assets/icons/blue/file.svg");
        background-repeat: no-repeat;
        background-size: 24px;
        padding-left: 26px;
        line-height: 24px; 
        margin-top: 4px;

        &.cw-fileicon-audio {
            background-image: url("../assets/icons/blue/file-audio.svg");
        }
        &.cw-fileicon-pic {
            background-image: url("../assets/icons/blue/file-pic.svg");
        }
        &.cw-fileicon-video {
            background-image: url("../assets/icons/blue/file-video.svg");
        }
        &.cw-fileicon-pdf {
            background-image: url("../assets/icons/blue/file-pdf.svg");
        }
        &.cw-fileicon-text {
            background-image: url("../assets/icons/blue/file-text.svg");
        }
        &.cw-fileicon-ppt {
            background-image: url("../assets/icons/blue/file-ppt.svg");
        }
        &.cw-fileicon-archive {
            background-image: url("../assets/icons/blue/file-archive.svg");
        }

        span {
            color: lightgrey;
        }
    }

    .cw-download-button-wrapper {
        text-align: right;
        margin-top: -2em;

        .cw-download-button {
            color: #28497c;
            border: solid thin#28497c;
            min-height: 27px;
            line-height: 130%;
            min-width: 100px;
            padding: 5px 15px 5px 30px;
            margin: 0 0 0.25em 0;
            cursor: pointer;
            box-sizing: border-box;
            text-align: center;
            text-decoration: none;
            vertical-align: bottom;
            white-space: nowrap;
            width: auto;
            height: 35px;
            background-position: 16px center;
            font-size: 14px;
            padding-left: 48px;
            padding-right: 20px;;
            display: inline-block;
            background-image: url("../assets/icons/blue/download.svg");
            background-color: #fff;
            background-repeat: no-repeat;

            &:hover{
                background-image: url("../assets/icons/white/download.svg");
                background-color:#28497c;
                color: #fff;
            }
        }
    }

}
</style>
