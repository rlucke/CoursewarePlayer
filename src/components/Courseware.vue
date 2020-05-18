<template>
    <div class="courseware-wrapper">
        <header class="courseware-header">
            {{ courseware.courseware_name }}
        </header>
        <div class="courseware">
            <ChapterNav 
                :chapters="courseware.children"
                :activeChapter="activeChapter.id" 
                :activeSubchapter="activeSubchapter.id"
                @selectSubchapter="setSubchapter"
                @navigate="navigate"
            />
            <Section 
                :subchapter="activeSubchapter"
                :coursewarePath="coursewarePath"
                :activeSection="activeSection"
                @goBack="goBack"
                @goForward="goForward"
                @navigate="navigate"
            />
        </div>
    </div>
</template>

<script>
import ChapterNav from './ChapterNav'
import Section from './Section'
// import axios from 'axios'
//import courseware from '../../cwData/courseware.json'

export default {
    name: 'Courseware',
    components: {
        ChapterNav,
        Section
    },
    data() {
        return{
            courseware: {},
            activeSubchapter: {},
            activeChapter: {},
            activeSection: {},
            coursewarePath: './cwData/'
        }
    },
    beforeMount() {
        //let view = this;
        // axios
        // .get(this.coursewarePath + 'courseware.json')
        // .then(response => {
        //     view.courseware = response.data;
        //     view.courseware.coursewarePath = view.coursewarePath;
        //     let firstChapter = view.courseware.children[0].id;
        //     view.setChapter(firstChapter);
        // })
        // .catch(error => {
        //     console.log(error);
        // });
        console.log(COURSEWARE);
        this.courseware = COURSEWARE;
        this.courseware.coursewarePath = this.coursewarePath;
        let firstChapter = this.courseware.children[0].id;
        this.setChapter(firstChapter);

    },
    methods: {
        navigate(nav){
            switch (nav.type) {
                case 'Section':
                    this.setSection(nav.id);
                    break;
                case 'Subchapter':
                    this.setSubchapter(nav.id);
                    break;
                case 'Chapter':
                    this.setChapter(nav.id);
                    break;
            }
        },
        setChapter(id){
            let view = this;
            this.courseware.children.forEach(chapter => {
                if(chapter.id == id) {
                    view.activeChapter = chapter;
                    view.activeSubchapter = chapter.children[0];
                    view.activeSection = view.activeSubchapter.children[0];
                }
            });
        },
        setSubchapter(id) {
            let view = this;
            this.courseware.children.forEach(chapter => {
                chapter.children.forEach(subchapter => {
                    if(subchapter.id == id) {
                        view.activeChapter = chapter;
                        view.activeSubchapter = subchapter;
                        view.activeSection = view.activeSubchapter.children[0];
                    }
                });
            });
        },
        setSection(id) {
            let view = this;
            this.courseware.children.forEach(chapter => {
                chapter.children.forEach(subchapter => {
                    subchapter.children.forEach(section => {
                        if(section.id == id) {
                            view.activeSubchapter = subchapter;
                            view.activeChapter = chapter;
                            view.activeSection = section;
                        }
                    });
                });
            });
        },
        goBack(){
            let view = this;
            if(this.activeSubchapter.id == this.activeChapter.children[0].id){
                // go to previous chapter if there is one
                if(this.activeChapter.id != this.courseware.children[0].id) {
                    this.courseware.children.forEach((chapter, index) => {
                        if(chapter.id == view.activeChapter.id) {
                            view.activeChapter = view.courseware.children[index-1];
                            view.activeSubchapter = view.activeChapter.children[0];
                            view.activeSection = view.activeSubchapter.children[0];
                        }
                    });
                }
            } else {
                this.activeChapter.children.forEach((subchapter, index) => {
                    if(view.activeSubchapter.id == subchapter.id) {
                        view.setSubchapter(view.activeChapter.children[index - 1].id);
                    }
                });
            }
        },
        goForward() {
            let view = this;
            let len = this.activeChapter.children.length;
            let id = '';
            if(this.activeSubchapter.id == this.activeChapter.children[len-1].id){
                // go to next chapter if there is one
                    this.courseware.children.forEach((chapter, index) => {
                        if(chapter.id == view.activeChapter.id) {
                            if (index < view.courseware.children.length-1) {
                                view.activeChapter = view.courseware.children[index+1];
                                view.activeSubchapter = view.activeChapter.children[0];
                                view.activeSection = view.activeSubchapter.children[0];
                            }
                        }
                    });
            } else {
                this.activeChapter.children.forEach((subchapter, index) => {
                    if(view.activeSubchapter.id == subchapter.id) {
                        id = view.activeChapter.children[index + 1].id;
                    }
                });
                this.setSubchapter(id);
            }
        }
    }
}
</script>
<style>
.courseware-wrapper {
    width: 1200px;
}
.courseware-header {
    font-size: 1.1em;
    padding: 1em;
    margin-bottom: 1em;
    color: #fff;
    background-color: #28497c;
}
.courseware {
    width: 1200px;
    display: flex;
    align-items: flex-start;
}
</style>
