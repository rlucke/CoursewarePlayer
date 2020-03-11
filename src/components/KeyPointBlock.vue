<template>
    <div class="cw-keypoint" :class="{['cw-keypoint-'+ block.data.keypoint_color]: true, ['cw-keypoint-icon-' + block.data.keypoint_icon]: true}" >
        <p class="cw-keypoint-sentence" >{{block.data.keypoint_content}}</p>
    </div>
</template>

<script>
export default {
    name: 'KeyPointBlock',
    props:{
        block: Object,
        coursewarePath: String
    }
}
</script>

<style lang="less">
@red: #cb1800;
@blue: #24437c;
@lightblue: #abb7ce;
@green: #00962d;
@grey: #6e6e6e;
@yellow: #ffad00;

.setIcon(@color) {
    &.cw-keypoint-icon-accept {
        background-image: url("../assets/icons/@{color}/accept.svg");
    }
    &.cw-keypoint-icon-add {
        background-image: url("../assets/icons/@{color}/add.svg");
    }
    &.cw-keypoint-icon-admin {
        background-image: url("../assets/icons/@{color}/admin.svg");
    }
    &.cw-keypoint-icon-billboard {
        background-image: url("../assets/icons/@{color}/billboard.svg");
    }
    &.cw-keypoint-icon-campusnavi {
        background-image: url("../assets/icons/@{color}/campusnavi.svg");
    }
    &.cw-keypoint-icon-chat2 {
        background-image: url("../assets/icons/@{color}/chat2.svg");
    }
    &.cw-keypoint-icon-code {
        background-image: url("../assets/icons/@{color}/code.svg");
    }
    &.cw-keypoint-icon-community {
        background-image: url("../assets/icons/@{color}/community.svg");
    }
    &.cw-keypoint-icon-computer {
        background-image: url("../assets/icons/@{color}/computer.svg");
    }
    &.cw-keypoint-icon-crown {
        background-image: url("../assets/icons/@{color}/crown.svg");
    }
    &.cw-keypoint-icon-consultation {
        background-image: url("../assets/icons/@{color}/consultation.svg");
    }
    &.cw-keypoint-icon-date {
        background-image: url("../assets/icons/@{color}/date.svg");
    }
    &.cw-keypoint-icon-decline {
        background-image: url("../assets/icons/@{color}/decline.svg");
    }
    &.cw-keypoint-icon-doctoral_cap {
        background-image: url("../assets/icons/@{color}/doctoral_cap.svg");
    }
    &.cw-keypoint-icon-dropbox {
        background-image: url("../assets/icons/@{color}/dropbox.svg");
    }
    &.cw-keypoint-icon-edit {
        background-image: url("../assets/icons/@{color}/edit.svg");
    }
    &.cw-keypoint-icon-exclaim {
        background-image: url("../assets/icons/@{color}/exclaim.svg");
    }
    &.cw-keypoint-icon-favorite {
        background-image: url("../assets/icons/@{color}/favorite.svg");
    }
    &.cw-keypoint-icon-filter {
        background-image: url("../assets/icons/@{color}/filter.svg");
    }
    &.cw-keypoint-icon-globe {
        background-image: url("../assets/icons/@{color}/globe.svg");
    }
    &.cw-keypoint-icon-graph {
        background-image: url("../assets/icons/@{color}/graph.svg");
    }
    &.cw-keypoint-icon-group2 {
        background-image: url("../assets/icons/@{color}/group2.svg");
    }
    &.cw-keypoint-icon-group3 {
        background-image: url("../assets/icons/@{color}/group3.svg");
    }
    &.cw-keypoint-icon-group4 {
        background-image: url("../assets/icons/@{color}/group4.svg");
    }
    &.cw-keypoint-icon-headache {
        background-image: url("../assets/icons/@{color}/headache.svg");
    }
    &.cw-keypoint-icon-info {
        background-image: url("../assets/icons/@{color}/info.svg");
    }
    &.cw-keypoint-icon-install {
        background-image: url("../assets/icons/@{color}/install.svg");
    }
    &.cw-keypoint-icon-institute {
        background-image: url("../assets/icons/@{color}/institute.svg");
    }
    &.cw-keypoint-icon-key {
        background-image: url("../assets/icons/@{color}/key.svg");
    }
    &.cw-keypoint-icon-learnmodule {
        background-image: url("../assets/icons/@{color}/learnmodule.svg");
    }
    &.cw-keypoint-icon-literature {
        background-image: url("../assets/icons/@{color}/literature.svg");
    }
    &.cw-keypoint-icon-lock-locked {
        background-image: url("../assets/icons/@{color}/lock-locked.svg");
    }
    &.cw-keypoint-icon-lock-unlocked {
        background-image: url("../assets/icons/@{color}/lock-unlocked.svg");
    }
    &.cw-keypoint-icon-mail {
        background-image: url("../assets/icons/@{color}/mail.svg");
    }
    &.cw-keypoint-icon-medal {
        background-image: url("../assets/icons/@{color}/medal.svg");
    }
    &.cw-keypoint-icon-metro {
        background-image: url("../assets/icons/@{color}/metro.svg");
    }
    &.cw-keypoint-icon-notification {
        background-image: url("../assets/icons/@{color}/notification.svg");
    }
    &.cw-keypoint-icon-notification2 {
        background-image: url("../assets/icons/@{color}/notification2.svg");
    }
    &.cw-keypoint-icon-outer-space {
        background-image: url("../assets/icons/@{color}/outer-space.svg");
    }
    &.cw-keypoint-icon-phone {
        background-image: url("../assets/icons/@{color}/phone.svg");
    }
    &.cw-keypoint-icon-place {
        background-image: url("../assets/icons/@{color}/place.svg");
    }
    &.cw-keypoint-icon-plugin {
        background-image: url("../assets/icons/@{color}/plugin.svg");
    }
    &.cw-keypoint-icon-question {
        background-image: url("../assets/icons/@{color}/question.svg");
    }
    &.cw-keypoint-icon-ranking {
        background-image: url("../assets/icons/@{color}/ranking.svg");
    }
    &.cw-keypoint-icon-remove {
        background-image: url("../assets/icons/@{color}/remove.svg");
    }
    &.cw-keypoint-icon-resources {
        background-image: url("../assets/icons/@{color}/resources.svg");
    }
    &.cw-keypoint-icon-roles {
        background-image: url("../assets/icons/@{color}/roles.svg");
    }
    &.cw-keypoint-icon-settings {
        background-image: url("../assets/icons/@{color}/settings.svg");
    }
    &.cw-keypoint-icon-span-empty {
        background-image: url("../assets/icons/@{color}/span-empty.svg");
    }
    &.cw-keypoint-icon-span-1quarter {
        background-image: url("../assets/icons/@{color}/span-1quarter.svg");
    }
    &.cw-keypoint-icon-span-2quarter {
        background-image: url("../assets/icons/@{color}/span-2quarter.svg");
    }
    &.cw-keypoint-icon-span-3quarter {
        background-image: url("../assets/icons/@{color}/span-3quarter.svg");
    }
    &.cw-keypoint-icon-span-full {
        background-image: url("../assets/icons/@{color}/span-full.svg");
    }
    &.cw-keypoint-icon-staple {
        background-image: url("../assets/icons/@{color}/staple.svg");
    }
    &.cw-keypoint-icon-star {
        background-image: url("../assets/icons/@{color}/star.svg");
    }
    &.cw-keypoint-icon-tools {
        background-image: url("../assets/icons/@{color}/tools.svg");
    }
    &.cw-keypoint-icon-topic {
        background-image: url("../assets/icons/@{color}/topic.svg");
    }
    &.cw-keypoint-icon-visibility-visible {
        background-image: url("../assets/icons/@{color}/visibility-visible.svg");
    }
    &.cw-keypoint-icon-vote {
        background-image: url("../assets/icons/@{color}/vote.svg");
    }


    &.cw-keypoint-icon-light-bulb {
        background-image: url("../assets/icons/@{color}/light_bulb.svg");
    }
}

.cw-keypoint-icon-option {
    .setIcon('blue');
    width: 16px;
    height: 16px;
    display: inline-block;
    vertical-align: middle;
    background-repeat: no-repeat;
    padding-right: 0.5em;
}

.cw-keypoint-red {
    border-color: @red;
    .setIcon('red');
}

.cw-keypoint-blue {
    border-color: @blue;
    .setIcon('blue');
}

.cw-keypoint-green {
    border-color: @green;
    .setIcon('green');
}

.cw-keypoint-grey {
    border-color: @grey;
    .setIcon('grey');
}

.cw-keypoint-yellow {
    border-color: @yellow;
    .setIcon('yellow');
}

.cw-keypoint-bg-red {
    background-color: @red;
}

.cw-keypoint-bg-blue {
    background-color: @blue;
}

.cw-keypoint-bg-green {
    background-color: @green;
}

.cw-keypoint-bg-grey {
    background-color: @grey;
}

.cw-keypoint-bg-yellow {
    background-color: @yellow;
}

.cw-keypoint {
    background-repeat: no-repeat;
    background-size: 48px;
    background-position-x: 2em;
    background-position-y: center;
    min-height: 52px;
    padding: 1.5em 2.5em 1.5em 8em;
    border-style: solid;
    border-width: 2px;
}

.cw-keypoint-sentence {
    font-size: 1.25em;
}

.cw-keypoint-select-icon option {
    padding-left: 2em;
    background-repeat: no-repeat;
}

.cw-keypoint-label-color {
    width: 32px !important;
    min-width: 32px !important;
    height: 32px;
    padding: 5px !important;

    > .cw-keypoint-input-color {
        visibility: hidden; 
        position: absolute;
    }

    > .cw-keypoint-input-color + div{ 
        cursor:pointer;
        border:2px solid transparent;
        height: 32px;
        width: 32px;
    }
}

.cw-keypoint-label-wrapper-color,
.cw-keypoint-label-wrapper-icon {
    padding: 6px 0 6px 5px;
    width: 30em;
    display: inline-block;
}

.cw-keypoint-label-wrapper-color {
    padding-left: 0.7em;
}

.cw-keypoint-label-wrapper-icon {
    margin-left: -0.5em;
}

label[for="cw-keypoint-icon"],
label[for="cw-keypoint-color"] {
    vertical-align: top;
}

.cw-keypoint-label-icon {
    width: 4em !important;
    min-width: 4em !important;
    padding: 0.5em !important;
    
    > .cw-keypoint-input-icon {
        visibility: hidden; 
        position: absolute;
    }

    > .cw-keypoint-input-icon + div{ 
        cursor:pointer;
        border:2px solid transparent;
        background-size: 24px 24px;
        background-repeat: no-repeat;
        background-position: top center;
        width: 5em;
        height: 3em;
        line-height: 5em;
        text-align: center;
        font-size: 0.9em;
        color: @lightblue;
    }
}
</style>