<template>
  <div class="EmbedBlock">
    <div v-html="html" ref="html"></div>
    <div class="cw-embedblock-info">
        <span class="cw-oembed-title">{{block.data.oembed.title}}</span>
        <span class="cw-embedblock-author-name">erstellt von {{block.data.oembed.author_name}}</span>
        <span class="cw-embedblock-source"> veröffentlicht auf {{block.data.oembed.provider_name}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EmbedBlock',
  props:{
    block: Object,
    coursewarePath: String
  },
  data(){
    return{
      html: ''
    }
  },
  watch:{
    html: function() {
      let iframe_height = this.$refs.html.children[0].height;
      let iframe_width = this.$refs.html.children[0].width;
      if (isNaN(iframe_height) || isNaN(iframe_width)) {
        this.$refs.html.children[0].height = Math.round(890 / 1.65);
      } else {
        this.$refs.html.children[0].height = (iframe_height / iframe_width) * 890;
      }
    }
  },
  beforeMount(){
    this.html = this.block.data.html;
  }
}
</script>

<style lang="less">
.EmbedBlock {
  .cw-embedblock-iframe {
    width: 890px !important;
  }
  .cw-embedblock-image.full-width{
    width: 890px;
    height: auto;
  }
  .cw-embedblock-info {
    color: #ccc;
  }
}
</style>