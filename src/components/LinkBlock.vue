<template>
  <div class="LinkBlock">
    <div v-if="external">
        <a :href="block.data.link_target" target="_blank">
            <div class="cw-link external">
                <span class="cw-link-title">{{block.data.link_title}}</span>
            </div>
        </a>
    </div>
    <div v-if="internal">
        <div class="cw-link internal" @click="navigateTo">
            <span class="cw-link-title">{{block.data.link_title}}</span>
        </div>
    </div>
  </div>
</template>

<script>

export default {
    name: 'LinkBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    computed: {
        external: function() {
            return this.block.data.link_type == 'external';
        },
        internal: function() {
            return this.block.data.link_type == 'internal';
        }
    },
    methods: {
        navigateTo() {
            let type = this.block.data.target_type;
            let id = this.block.data.target_id;

            this.$emit('navigate', {type: type, id: id});
        }
    }
}
</script>

<style lang="less">
.LinkBlock {
    &:hover {
        cursor: pointer;
    }
    a {
        text-decoration: none;
    }
    .cw-link {
        padding: 1em 1em 1em 5.5em;
        border: solid thin #ccc;
        color:#28497c;

        &:hover {
            border: solid thin #28497c;
            color: #fff;
            background-color: #28497c;
        }

        &.internal {
            background-image: url("../assets/icons/blue/link-intern.svg");
            background-position: 2em 50%;
            background-repeat: no-repeat;
            background-size: 28px;

            .cw-link-progress-true,
            .cw-link-progress-false {
                background-repeat: no-repeat;
                background-size: 16px;
                height: 16px;
                width: 16px;
                display: inline-block;
            }

            &:hover {
                background-image: url("../assets/icons/white/link-intern.svg");
            }
        }

        &.external {
            background-image: url("../assets/icons/blue/link-extern.svg");
            background-position: 2em 50%;
            background-repeat: no-repeat;
            background-size: 28px;
            &:hover {
                background-image: url("../assets/icons/white/link-extern.svg");
            }

            .cw-link-og-image {
                display: inline-block;
                max-width: 40%;
                vertical-align: top;
                margin-right: 2em;
            }
            .cw-link-og-textwrapper {
                display: inline-block;
                max-width: 50%;
                p {
                    margin: 0;
                }
                .cw-link-og-site {
                    font-size: 1.25em;
                }
                .cw-link-og-title {
                    font-weight: 600;
                }
                .cw-link-og-description {
                    color:#687fa3;
                    text-align: justify;
                }
            }
        }
    }
}
</style>
