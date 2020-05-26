<template>
    <nav class="chapternav">
        <ul class="chapter-list">
            <div v-for="chapter in chapters" :key="chapter.id">
                <li 
                    class="chapter-item"
                    :class="{ 'active-item': activeChapter == chapter.id }"
                    @click="navigate('Chapter', chapter.id)"
                >
                    {{ chapter.title }}
                </li>
                <div v-if="chapter.children.length && (activeChapter == chapter.id)">
                    <li
                        class="subchapter-item" 
                        v-for="subchapter in chapter.children" :key="subchapter.id"
                        :class="{ 'active-item': activeSubchapter == subchapter.id }"
                        @click="navigate('Subchapter', subchapter.id)"
                    >
                        {{ subchapter.title }}
                    </li>
                </div>
            </div>
        </ul>
    </nav>
</template>

<script>
export default {
    name: 'ChapterNav',
    props: {
        chapters: Array,
        activeChapter: String,
        activeSubchapter: String
    },
    data() {
        return {
            // selectedChapter: ''
        }
    },
    watch:{
        // activeChapter: function(value) {
        //     this.selectedChapter = value;
        // },
        // activeSubchapter: function(value){
        //     this.selectedSubchapter = value;
        // },
        // chapters: function(value) {
            // this.selectedChapter = value[0].id;
        //     this.selectedSubchapter = value[0].children[0].id;
        // }
    },
    methods: {
        navigate(type, id){
            this.$emit('navigate', {type: type, id: id});
        }
    }
}
</script>
<style>
    .chapternav {
        width: 280px;
        border: 1px solid #d0d7e3;
        margin: 0;
        background-color: #fff;
        color: #28497c;
    }
    .chapternav-header {
        border: none;
        background-color: #e7ebf1;
        font-weight: 600;
        text-align: left;
        padding: 4px;
        margin: 0;
    }
    .chapter-list {
        padding: 0;
        margin: 0; 
    }
    .chapter-item {
        background-image: url("../assets/icons/blue/arr_1right.svg");
        background-repeat: no-repeat;
        background-position-x: left+0.5em;
        background-position-y: center;
        margin: 0;
        padding: 0.5em 0 0.5em 2em;
        list-style: none;
    }
    .chapter-item.active-item {
        background-image: url("../assets/icons/blue/arr_1down.svg");
        background-color: #e4e4e4;
        font-weight: 600;
    }
    .subchapter-item.active-item {
        background-color: #f0f0f0;
        font-weight: 600;
    }
    .subchapter-item {
        margin: 0;
        padding: 0.5em 0 0.5em 3em;
        list-style: none;
    }
    .subchapter-item:hover, 
    .chapter-item:hover {
        color: #d60000;
        cursor: pointer;
    }
</style>
