<template>
  <div class="block html-block" v-html="html">
  </div>
</template>

<script>
export default {
    name: 'HtmlBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data(){
        return {
            html: String
        }
    },
    beforeMount() {
        this.html = this.block.data;
        this.html = this.html.replace("./", this.coursewarePath);
    }
}
</script>

<style>
.block {
    width: 100%;
    margin-bottom: 1em;
}
</style>
