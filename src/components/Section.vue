<template>
    <div class="section">
        <SectionNav
            :selected="activeSection.id"
            :sections="subchapter.children"
            @setSection="setSection"
            @goBack="goBack"
            @goForward="goForward"
        />
        <header class="section-header">{{ activeSection.title }}</header>
        <ul class="block-list">
            <li class="block-list-item" v-for="block in blocks" :key="block.id">
                <component :is="block.type" :block="block" :SectionBlocks="blocks" :coursewarePath="coursewarePath" @navigate="navigate" @hideBlocks="hideBlocks"></component>
            </li>
        </ul>
    </div>
</template>

<script>
import SectionNav from './SectionNav';

import AssortBlock from './AssortBlock';
import AudioBlock from './AudioBlock';
import BeforeAfterBlock from './BeforeAfterBlock';
import CanvasBlock from './CanvasBlock';
import ChartBlock from './ChartBlock';
import CodeBlock from './CodeBlock';
import DateBlock from './DateBlock';
import DialogCardsBlock from './DialogCardsBlock';
import DownloadBlock from './DownloadBlock';
import EmbedBlock from './EmbedBlock';
import FolderBlock from './FolderBlock';
import GalleryBlock from './GalleryBlock';
import HtmlBlock from './HtmlBlock';
import IFrameBlock from './IFrameBlock';
import ImageMapBlock from './ImageMapBlock';
import InteractiveVideoBlock from './InteractiveVideoBlock';
import KeyPointBlock from './KeyPointBlock';
import LinkBlock from './LinkBlock';
import PdfBlock from './PdfBlock';
// import TestBlock from './TestBlock';
import TypewriterBlock from './TypewriterBlock';
import VideoBlock from './VideoBlock';

export default {
    name: 'Section',
    components: {
        SectionNav, AssortBlock,
        AudioBlock, BeforeAfterBlock, CanvasBlock, ChartBlock,
        CodeBlock, DateBlock, DialogCardsBlock, DownloadBlock, EmbedBlock,
        FolderBlock, GalleryBlock, HtmlBlock, IFrameBlock,
        ImageMapBlock, InteractiveVideoBlock,KeyPointBlock, LinkBlock,
        PdfBlock, TypewriterBlock, VideoBlock,
        // TestBlock, 
    },
    props: {
        subchapter: Object,
        coursewarePath: String,
        activeSection: Object
    },
    data(){
        return{
            blockList: [],
            hideBlockList: []
        }
    },
    computed: {
        blocks() {
            let view = this;
            let blocks = [];
            if(this.activeSection.children == null) {return blocks;}
            this.activeSection.children.forEach(block =>{
                if((view.blockList.includes(block.type)) && (!view.hideBlockList.includes(block.id))){
                    blocks.push(block);
                }
            });

            return blocks;
        }
    },
    beforeMount(){
        for (let [key, value] of Object.entries(this.$options.components)) {
            if (key.indexOf('Block') > -1) {
                this.blockList.push(key);
            }
        }
    },
    methods:{
        setSection(sectionId) {
            this.navigate({type: 'Section', id: sectionId});
        },
        goBack(){
            let view = this;
            if(this.subchapter.children[0].id == this.activeSection.id){
                this.$emit('goBack');
            } else{
                this.subchapter.children.forEach((section, index) => {
                if (section.id == view.activeSection.id) {
                        let sectionId = view.subchapter.children[index-1].id;
                        view.navigate({type: 'Section', id: sectionId});
                }
                });
            }
        },
        goForward() {
            let view = this;
            let len = this.subchapter.children.length;
            if(this.subchapter.children[len-1].id == this.activeSection.id){
                this.$emit('goForward');
            } else{
                this.subchapter.children.forEach((section, index) => {
                    if (section.id == view.activeSection.id) {
                        let sectionId = view.subchapter.children[index+1].id;
                        view.navigate({type: 'Section', id: sectionId});
                    }
                });
            }
        },
        navigate(event) {
            this.$emit('navigate', event);
        },
        hideBlocks(blocks) {
            let view = this;
            blocks.forEach((id)=> {
                view.hideBlockList.push(id);
            });
        }
    }
}
</script>

<style>
.section {
    width: 900px;
    margin-left: 30px;
}
.section-header {
    font-size: 2em;
    color: #000;
    font-weight: 600;
    margin-bottom: 14px;
}
.block-list {
    margin: 0;
    padding: 0;
}
.block-list-item {
    list-style: none;
    margin-bottom: 1em;
}
.block-header {
    min-width: 10%;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 100%;
    font-style: normal;
    font-weight: 600;
    color: #28497c;
    font-size: 16px;
    line-height: 20px;
    padding: 10px 20px;
    position: relative;
    margin: 0;
    text-align: center;
    border: solid thin #ccc;
    border-bottom: none;
    background-color: #fcfcfc;
}
</style>
