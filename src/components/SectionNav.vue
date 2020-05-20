<template>
    <ul class="section-nav" v-if="sections">
        <li class="section-nav-back" @click="$emit('goBack')"></li>
        <li 
            class="section-nav-item"
            v-for="section in sections"
            :key="section.id"
            :title="section.title"
            :class="[section.id == selectedSection ? 'active' : '', 'icon-'+section.icon]"
            @click="selectSection(section.id)"
        >
        </li>
        <li class="section-nav-forward" @click="$emit('goForward')"></li>
    </ul>
</template>

<script>
export default {
    name: 'SectionNav',
    props: {
        selected: String,
        sections: Array
    },
    data() {
        return{
            selectedSection: '' //section.id
        }
    },
    beforeMount() {
        this.selectedSection = this.selected;
    },
    methods:{
        selectSection(id) {
            this.selectedSection = id;
            this.$emit('setSection', id);
        },
    }
}
</script>
<style lang="less">
.section-nav {
    margin: 0;
    padding: 0;
    min-width: 80px;
    height: 40px;
    margin-bottom: 1.5em;
}
.section-nav-item {
    border: solid thin rgba(40, 73, 124, 0.4);
    margin: 0 0.5em;
}
.section-nav-back,
.section-nav-forward{
    border: solid thin transparent;
}
.section-nav-back,
.section-nav-forward,
.section-nav-item {
    width: 35px;
    height: 35px;
    padding: 0 0.5em;
    display: inline-block;
    overflow: hidden;
    background-size: 24px;
    background-repeat: no-repeat;
    background-position-x: center;
    background-position-y: center;
}
.section-nav-back:hover,
.section-nav-forward:hover,
.section-nav-item:hover,
.section-nav-item.active {
    cursor: pointer;
    background-color: #28497c;
    border: solid thin #28497c;
 }
.icon-chat {
    background-image: url("../assets/icons/blue/chat.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/chat.svg");
    }
}
.icon-document {
    background-image: url("../assets/icons/blue/info.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/info.svg");
    }
}
.icon-task {
    background-image: url("../assets/icons/blue/question.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/question.svg");
    }
}
.icon-video {
    background-image: url("../assets/icons/blue/video.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/video.svg");
    }
}
.icon-audio {
    background-image: url("../assets/icons/blue/play.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/play.svg");
    }
}
.icon-code {
    background-image: url("../assets/icons/blue/computer.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/computer.svg");
    }
}
.icon-search {
    background-image: url("../assets/icons/blue/search.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/search.svg");
    }
}
.icon-opencast {
    background-image: url("../assets/icons/blue/opencast.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/opencast.svg");
    }
}
.icon-light-bulb {
    background-image: url("../assets/icons/blue/light_bulb.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/light_bulb.svg");
    }
}
.icon-gallery {
    background-image: url("../assets/icons/blue/picture.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/picture.svg");
    }
}
.icon-graph {
    background-image: url("../assets/icons/blue/graph.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/graph.svg");
    }
}
.icon-action {
    background-image: url("../assets/icons/blue/action.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/action.svg");
    }
}
.icon-accept {
    background-image: url("../assets/icons/blue/accept.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/accept.svg");
    }
}
.icon-admin {
    background-image: url("../assets/icons/blue/admin.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/admin.svg");
    }
}
.icon-billboard {
    background-image: url("../assets/icons/blue/billboard.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/billboard.svg");
    }
}
.icon-campusnavi {
    background-image: url("../assets/icons/blue/campusnavi.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/campusnavi.svg");
    }
}
.icon-category {
    background-image: url("../assets/icons/blue/category.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/category.svg");
    }
}
.icon-cloud2 {
    background-image: url("../assets/icons/blue/cloud2.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/cloud2.svg");
    }
}
.icon-comment {
    background-image: url("../assets/icons/blue/comment.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/comment.svg");
    }
}
.icon-community {
    background-image: url("../assets/icons/blue/community.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/community.svg");
    }
}
.icon-date {
    background-image: url("../assets/icons/blue/date.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/date.svg");
    }
}
.icon-doctoral_cap {
    background-image: url("../assets/icons/blue/doctoral_cap.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/doctoral_cap.svg");
    }
}
.icon-edit-small {
    background-image: url("../assets/icons/blue/edit-small.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/edit-small.svg");
    }
}
.icon-exclaim {
    background-image: url("../assets/icons/blue/exclaim.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/exclaim.svg");
    }
}
.icon-favorite {
    background-image: url("../assets/icons/blue/favorite.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/favorite.svg");
    }
}
.icon-file {
    background-image: url("../assets/icons/blue/file.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/file.svg");
    }
}
.icon-filter {
    background-image: url("../assets/icons/blue/filter.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/filter.svg");
    }
}
.icon-folder-empty2 {
    background-image: url("../assets/icons/blue/folder-empty2.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/folder-empty2.svg");
    }
}
.icon-globe {
    background-image: url("../assets/icons/blue/globe.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/globe.svg");
    }
}
.icon-group2 {
    background-image: url("../assets/icons/blue/group2.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/group2.svg");
    }
}
.icon-group4 {
    background-image: url("../assets/icons/blue/group4.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/group4.svg");
    }
}
.icon-guestbook {
    background-image: url("../assets/icons/blue/guestbook.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/guestbook.svg");
    }
}
.icon-home {
    background-image: url("../assets/icons/blue/home.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/home.svg");
    }
}
.icon-institute {
    background-image: url("../assets/icons/blue/institute.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/institute.svg");
    }
}
.icon-key {
    background-image: url("../assets/icons/blue/key.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/key.svg");
    }
}
.icon-literature {
    background-image: url("../assets/icons/blue/literature.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/literature.svg");
    }
}
.icon-license {
    background-image: url("../assets/icons/blue/license.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/license.svg");
    }
}
.icon-lock-locked {
    background-image: url("../assets/icons/blue/lock-locked.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/lock-locked.svg");
    }
}
.icon-lock-unlocked {
    background-image: url("../assets/icons/blue/lock-unlocked.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/lock-unlocked.svg");
    }
}
.icon-news {
    background-image: url("../assets/icons/blue/news.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/news.svg");
    }
}
.icon-notification2 {
    background-image: url("../assets/icons/blue/notification2.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/notification2.svg");
    }
}
.icon-phone {
    background-image: url("../assets/icons/blue/phone.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/phone.svg");
    }
}
.icon-place {
    background-image: url("../assets/icons/blue/place.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/place.svg");
    }
}
.icon-plugin {
    background-image: url("../assets/icons/blue/plugin.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/plugin.svg");
    }
}
.icon-print {
    background-image: url("../assets/icons/blue/print.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/print.svg");
    }
}
.icon-ranking {
    background-image: url("../assets/icons/blue/ranking.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/ranking.svg");
    }
}
.icon-refresh {
    background-image: url("../assets/icons/blue/refresh.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/refresh.svg");
    }
}
.icon-resources {
    background-image: url("../assets/icons/blue/resources.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/resources.svg");
    }
}
.icon-span-empty {
    background-image: url("../assets/icons/blue/span-empty.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/span-empty.svg");
    }
}
.icon-span-1quarter {
    background-image: url("../assets/icons/blue/span-1quarter.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/span-1quarter.svg");
    }
}
.icon-span-2quarter {
    background-image: url("../assets/icons/blue/span-2quarter.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/span-2quarter.svg");
    }
}
.icon-span-3quarter {
    background-image: url("../assets/icons/blue/span-3quarter.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/span-3quarter.svg");
    }
}
.icon-span-full {
    background-image: url("../assets/icons/blue/span-full.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/span-full.svg");
    }
}
.icon-sport {
    background-image: url("../assets/icons/blue/sport.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/sport.svg");
    }
}
.icon-staple {
    background-image: url("../assets/icons/blue/staple.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/staple.svg");
    }
}
.icon-star {
    background-image: url("../assets/icons/blue/star.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/star.svg");
    }
}
.icon-stat {
    background-image: url("../assets/icons/blue/stat.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/stat.svg");
    }
}
.icon-studygroup {
    background-image: url("../assets/icons/blue/studygroup.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/studygroup.svg");
    }
}
.icon-tag {
    background-image: url("../assets/icons/blue/tag.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/tag.svg");
    }
}
.icon-tools {
    background-image: url("../assets/icons/blue/tools.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/tools.svg");
    }
}
.icon-vote {
    background-image: url("../assets/icons/blue/vote.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/vote.svg");
    }
}
.icon-wizard {
    background-image: url("../assets/icons/blue/wizard.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/wizard.svg");
    }
}
.icon-youtube {
    background-image: url("../assets/icons/blue/youtube.svg");
    &:hover, &.active {
         background-image: url("../assets/icons/white/youtube.svg");
    }
}

.section-nav-back {
    background-image: url("../assets/icons/blue/arr_1left.svg");
}
.section-nav-forward {
    background-image: url("../assets/icons/blue/arr_1right.svg");
}
.section-nav-back:hover {
    background-image: url("../assets/icons/white/arr_1left.svg");
}
.section-nav-forward:hover {
    background-image: url("../assets/icons/white/arr_1right.svg");
}
</style>
