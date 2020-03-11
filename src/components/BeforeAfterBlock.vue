<template>
    <div class="BeforeAfterBlock">
        <TwentyTwenty
            :before="beforeFilePath"
            :after="afterFilePath"
            :beforeLabel="block.data.ba_before.file_name"
            :afterLabel="block.data.ba_after.file_name"
        />
    </div>
</template>

<script>
import TwentyTwenty from 'vue-twentytwenty';

export default {
    name: 'BeforeAfterBlock',
    components: {
        TwentyTwenty
    },
    props:{
        block: Object,
        coursewarePath: String
    },
    computed: {
        beforeFilePath() {
            if(this.block.data.ba_before.source == 'file') {
                return this.coursewarePath + this.block.data.ba_before.url;
            } else {
                return this.block.data.ba_before.url;
            }
        },
        afterFilePath() {
            if(this.block.data.ba_after.source == 'file') {
                return this.coursewarePath + this.block.data.ba_after.url;
            } else {
                return this.block.data.ba_after.url;
            }
        }
    }
}
</script>