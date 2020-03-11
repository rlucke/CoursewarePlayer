<template>
    <div class="CodeBlock">
        <div class="code-content content">
            <pre><code ref="code" :class="[block.data.code_lang]">{{block.data.code_content}}</code></pre>
            <div class="code-lang">
                <span>{{block.data.code_lang}}</span>
            </div>
        </div>
    </div>
</template>

<script>
import hljs from 'highlight.js'
export default {
    name: 'CodeBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    mounted() {
        hljs.highlightBlock(this.$refs.code);
    }
}
</script>

<style lang="less">
.CodeBlock {
    pre {
        margin-bottom: 0;
    }

    .hljs {
      display: block;
      overflow-x: auto;
      padding: 0.5em;
      background: #fcfcfc;
      color: black;
      border: solid thin #f3f3f3;
    }

    .hljs-comment,
    .hljs-quote,
    .hljs-variable {
      color: #008000;
    }

    .hljs-keyword,
    .hljs-selector-tag,
    .hljs-selector-class,
    .hljs-built_in,
    .hljs-name,
    .hljs-tag {
      color: #24437c;
      font-weight: 600;
    }

    .hljs-string,
    .hljs-title,
    .hljs-section,
    .hljs-attribute,
    .hljs-literal,
    .hljs-template-tag,
    .hljs-template-variable,
    .hljs-type,
    .hljs-addition {
      color: #f5871f;
      font-weight: 400;
    }

    .hljs-deletion,
    .hljs-selector-attr,
    .hljs-selector-pseudo,
    .hljs-meta {
      color: #2b91af;
      font-weight: 400;
    }

    .hljs-doctag {
      color: #808080;
      font-weight: 400;
    }

    .hljs-attr {
      color: #f00;
      font-weight: 400;
    }

    .hljs-symbol,
    .hljs-bullet,
    .hljs-link {
      color: #00b0e8;
      font-weight: 400;
    }

    .hljs-emphasis {
      font-style: italic;
      font-weight: 400;
    }

    .hljs-strong {
      font-weight: 600;
    }

    .code-lang {
        background: #f7f7f7;
        border: solid thin #f3f3f3;
        padding: 5px 10px;
        text-align: right;
        color: #aaa;
        font-family: monospace;
        text-transform: full-width;
    }
}
</style>
