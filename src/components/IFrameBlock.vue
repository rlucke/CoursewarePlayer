<template>
  <div class="IFrameBlock">
        <iframe 
            width=100% 
            :height="block.data.height"
            :src="block.data.url"
            allowfullscreen
            sandbox="allow-forms allow-popups allow-pointer-lock allow-same-origin allow-scripts"
        />
        <div v-if="block.data.cc_infos" class="iframe-cc-data">
            <div class="iframe-cc-icon" :class="[block.data.cc_infos.cc_type]"></div>
            <div class="iframe-cc-infos-student">
                <span v-if="block.data.cc_infos.cc_work_name">
                    Werk: 
                    <a v-if="block.data.cc_infos.cc_work_url" :href="block.data.cc_infos.cc_work_url" target="_blank">{{block.data.cc_infos.cc_work_name}}</a>
                    <div v-else class="iframe-cc-infos-text">{{block.data.cc_infos.cc_work_name}}</div>
                </span>
                <span v-if="block.data.cc_infos.cc_author_name">
                    Autor: 
                    <a v-if="block.data.cc_infos.cc_author_url" :href="block.data.cc_infos.cc_author_url" target="_blank">{{block.data.cc_infos.cc_author_name}}</a>
                    <div v-else class="iframe-cc-infos-text">{{block.data.cc_infos.cc_author_name}}</div>
                </span>
                <span v-if="block.data.cc_infos.cc_license_name">
                    Lizenz der Plattform: 
                    <a v-if="block.data.cc_infos.cc_license_url" :href="block.data.cc_infos.cc_license_url" target="_blank">{{block.data.cc_infos.cc_license_name}}</a>
                    <div v-else class="iframe-cc-infos-text">{{block.data.cc_infos.cc_license_name}}</div>
                </span>
                <br>
            </div>
        </div>
  </div>
</template>

<script>
export default {
    name: 'IFrameBlock',
    props:{
        block: Object,
        coursewarePath: String
    }
}
</script>

<style lang="less">
.IFrameBlock {
    iframe {
        border: solid thin #ccc;
    }
    .iframe-cc-icon {
        width: 120px;
        height: 50px;
        display: inline-block;
        background-repeat: no-repeat;
        margin-left: 2em;
        vertical-align: top;

        &.default {
            background-image: url("../assets/icons/cc/cc.srr.primary.svg");
        }
        &.by {
            background-image: url("../assets/icons/cc/by.svg");
        }
        &.by-nc {
            background-image: url("../assets/icons/cc/by-nc.eu.svg");
        }
        &.by-nc-nd {
            background-image: url("../assets/icons/cc/by-nc-nd.eu.svg");
        }
        &.by-nc-sa {
            background-image: url("../assets/icons/cc/by-nc-sa.svg");
        }
        &.by-nd {
            background-image: url("../assets/icons/cc/by-nd.svg");
        }
        &.by-sa {
            background-image: url("../assets/icons/cc/by-sa.svg");
        }
    }

    .iframe-cc-data {
        border: solid thin #ccc;
        margin-top: -5px;
        padding-top: 1em;
        width: 100%;
    }
    .iframe-cc-infos-student {
        display: inline-block;
        padding-left: 2em;

        span {
            display: block;
        }
        .iframe-cc-infos-text {
            display: inline;
        }
    }

}
</style>
