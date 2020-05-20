<template>
  <div class="AssortBlock">
      <ul v-if="assortType == 'accordion'" class="assort-block-accordion">
          <li class="assort-block-item" v-for="block in assortBlocks" :key="block.id">
              <h1 
                class="assort-block-header"
                :class="[block.unfold == true ? 'unfold' : '']"
                :block-id="block.id"
                @click="unfoldItem"
                >
                    {{block.name}}
              </h1>
              <div class="assort-block-content" :class="[block.unfold == true ? 'unfold' : '']">
                <component :is="block.ui_block.type" :block="block.ui_block" :coursewarePath="coursewarePath"></component>
              </div>
          </li>
      </ul>
      <div v-if="assortType == 'tabs'" class="assort-block-tabs">
        <ul class="assort-block-tabs-nav">
            <li class="assort-block-item" v-for="block in assortBlocks" :key="block.id">
                <h1 
                    class="assort-block-header"
                    :class="[block.unfold == true ? 'unfold' : '']"
                    :block-id="block.id"
                    @click="unfoldItem"
                    >
                        {{block.name}}
                </h1>
            </li>
          </ul>
          <div class="assort-block-content" :class="[block.unfold == true ? 'unfold' : '']" v-for="block in assortBlocks" :key="block.id">
            <component :is="block.ui_block.type" :block="block.ui_block" :coursewarePath="coursewarePath"></component>
          </div>
        </div>

  </div>
</template>

<script>
import HtmlBlock from './HtmlBlock';
import VideoBlock from './VideoBlock';
import IFrameBlock from './IFrameBlock';
import DownloadBlock from './DownloadBlock';
import KeyPointBlock from './KeyPointBlock';
import LinkBlock from './LinkBlock';
import EmbedBlock from './EmbedBlock';
import FolderBlock from './FolderBlock';

export default {
    name: 'AssortBlock',
    components: {
        HtmlBlock, VideoBlock, IFrameBlock, DownloadBlock, KeyPointBlock, LinkBlock, EmbedBlock, FolderBlock
    },
    props:{
        block: Object,
        SectionBlocks: Array,
        coursewarePath: String
    },
    data(){
        return {
            assortBlocks: [],
            assortType: ''
        }
    },
    beforeMount() {
        let view = this;
        this.assortType = this.block.data.assorttype;
        this.assortBlocks = this.block.data.assortblocks;
        let hideBlockList = [];
        this.assortBlocks.forEach((block, index)=> {
            block.unfold = false;
            if (index == 0) {
                block.unfold = true;
            }
            hideBlockList.push(block.id);
            view.SectionBlocks.forEach((ui_block) => {
                if (block.id == ui_block.id) {
                    block.ui_block = ui_block;
                }
            });
        });
        this.$emit('hideBlocks', hideBlockList);
    },
    methods:{
        unfoldItem(event){
            let blockId = event.target.getAttribute('block-id');
            let blocks = this.assortBlocks;
            this.assortBlocks = [];
            blocks.forEach((block) => {
                if (block.id == blockId) {
                    block.unfold = true;
                } else {
                    block.unfold = false;
                }
            });
            this.assortBlocks = blocks;
        }
    }
}
</script>

<style lang="less">
.AssortBlock {
    .assort-block-accordion {
        list-style: none;
        padding: 0;
        .assort-block-item {
            border: solid thin #ccc;
            border-bottom: none;
            &:last-child{
                border-bottom: solid thin #ccc;
            }
            .assort-block-header {
                margin: 0;
                font-size: 1em;
                padding: 1em 1em 1em 2em;
                background-color: #eee;
                color: #28497c;
                cursor: pointer;
                background-repeat: no-repeat;
                background-position-x: 0.5em;
                background-position-y: center;
                background-image: url("../assets/icons/blue/arr_1right.svg");
                &.unfold {
                    background-image: url("../assets/icons/blue/arr_1down.svg");
                }
            }
            .assort-block-content {
                padding: 1em;
                display: none;

                &.unfold {
                    display: block;
                }
            }
        }
    }
    .assort-block-tabs {
        .assort-block-tabs-nav {
            display: inline-block;
            list-style: none;
            padding: 0.5em 0 0 1em;
            background-color: #e9e9e9;
            width: -moz-available;
            .assort-block-item {
                float: left;
                .assort-block-header {
                    margin: 0;
                    font-size: 1em;
                    padding: 1em;
                    background-color: #e9e9e9;
                    color: #28497c;
                    cursor: pointer;
                    &.unfold {
                        background-color: #fff;

                    }
                }
            }
        }
        .assort-block-content {
            border: solid thin #e9e9e9;
            border-top: none;
            margin-top: -3px;
            padding: 1em;
            display: none;
            &.unfold {
                display: block;
            }
        }
    }
}
</style>
