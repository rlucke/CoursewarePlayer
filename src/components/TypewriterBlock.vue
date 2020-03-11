<template>
  <div class="TypewriterBlock">
      <p
        ref="typewriter"
        class="cw-typewriter"
        :class="[block.data.font, block.data.size]"
        v-html="content"
    ></p>
  </div>
</template>

<script>
export default {
    name: 'TypewriterBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data(){
        return{
            typing: false,
            scrolledIntoView: false,
            speeds: [200,100,50,25],
            content: ''
        }
    },
    watch: {
        scrolledIntoView: function(val)  {
            if (val && !this.typing) {this.type();}
        }
    },
    mounted(){
        let view = this;
        this.isScrolledIntoView();
        window.addEventListener('scroll', function(e) {
            view.isScrolledIntoView();
        });
    },
    methods:{
        type() {
            let view = this;
            this.typing = true;
            this.block.data.content.split('').forEach((letter, index) => {
                setTimeout(function(){
                    view.content = view.content + letter;
                }, view.speeds[view.block.data.speed] * index);
            });
        },
        isScrolledIntoView() {
            this.scrolledIntoView = (
                (this.$refs.typewriter.offsetTop + this.$refs.typewriter.offsetHeight <= window.scrollY + window.innerHeight) 
                && (this.$refs.typewriter.offsetTop >= window.scrollY)
            );
        }
    }
}
</script>

<style lang="less">
.TypewriterBlock {
    .cw-typewriter {
        white-space: pre-wrap;

        &.font-typewriter{
            font-family: "Lucida Sans Typewriter", "Lucida Console", Monaco, "Bitstream Vera Sans Mono", monospace;
        }
        &.font-trebuchet {
            font-family: "Trebuchet MS", "Lucida Grande", "Lucida Sans Unicode", "Lucida Sans", sans-serif;
        }
        &.font-tahoma {
            font-family: Tahoma, Verdana, Segoe, sans-serif;
        }
        &.font-georgia {
            font-family: Georgia, Times, "Times New Roman", serif;
        }
        &.font-narrow {
            font-family: "Arial Narrow", Arial, "Helvetica Condensed", Helvetica, sans-serif;
        }

        &.size-tall {
            font-size: 1.5em;
            line-height: 1.25em;
        }
        &.size-grande {
            font-size: 2em;
            line-height: 1.25em;
        }
        &.size-huge {
            font-size: 4em;
            line-height: 1.25em;
        }
    }
}
</style>
