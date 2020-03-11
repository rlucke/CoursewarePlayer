<template>
  <div class="block html-block" v-html="block.data">
  </div>
</template>

<script>
export default {
    name: 'HtmlBlock',
    props:{
        block: Object,
        coursewarePath: String
    }
}
</script>

<style>
.block {
    width: 100%;
    margin-bottom: 1em;
}
</style>
