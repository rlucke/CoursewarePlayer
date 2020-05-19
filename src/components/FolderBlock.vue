<template>
    <div class="FolderBlock">
        <h1 class="block-header">{{block.data.folder_title}}</h1>
        <div class="cw-folder">
            <ul>
                <li v-for="file in block.data.files" :key="file.id" class="cw-folder-file-item">
                    <a :href="coursewarePath + file.path" :download="file.name" target="_blank">
                        <span class="cw-folder-file-info" :class="['cw-folder-fileicon-'+file.icon]"> {{file.name}} </span>
                        <span name="download" class="cw-folder-download-icon" ></span>
                    </a> 
            </li>
        </ul>
        </div>
  </div>
</template>

<script>
export default {
    name: 'FolderBlock',
    props:{
        block: Object,
        coursewarePath: String
    }
}
</script>

<style lang="less">
.FolderBlock{
    .cw-folder{
        border: solid thin #ccc;
        padding: 1em;
        ul {
            padding: 0;
        }
    }

    .cw-folder-file-item {
        list-style: none;
        
        &:not(:last-child) {
            border-bottom: solid thin #ccc;
        }
        a {
            display: block;
        }
        &:hover {
            background-color: hsla(217,6%,45%,.2);
        }
    }

    .cw-folder-file-info { 
        background-image: url("../assets/icons/blue/file.svg");
        display: inline-block;
        background-repeat: no-repeat;
        background-size: 24px;
        padding-left: 26px;
        margin: 1em;
        line-height: 24px; 
        color: #28497c;

        &.cw-folder-fileicon-audio {
            background-image: url("../assets/icons/blue/file-audio.svg");
        }
        &.cw-folder-fileicon-pic {
            background-image: url("../assets/icons/blue/file-pic.svg");
        }
        &.cw-folder-fileicon-video {
            background-image: url("../assets/icons/blue/file-video.svg");
        }
        &.cw-folder-fileicon-pdf {
            background-image: url("../assets/icons/blue/file-pdf.svg");
        }
        &.cw-folder-fileicon-text {
            background-image: url("../assets/icons/blue/file-text.svg");
        }
        &.cw-folder-fileicon-ppt {
            background-image: url("../assets/icons/blue/file-ppt.svg");
        }
        &.cw-folder-fileicon-archive {
            background-image: url("../assets/icons/blue/file-archive.svg");
        }
    }
    .cw-folder-download-icon {
        float: right;
        background-image: url("../assets/icons/blue/download.svg");
        height: 24px;
        width: 24px;
        background-size: 24px;
        background-repeat: no-repeat;
        margin: 1em;
    }
}
</style>
