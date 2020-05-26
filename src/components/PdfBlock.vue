<template>
    <div class="PdfBlock" v-if="PdfViewer">
        <div class="cw-pdf-header">
            <button 
                class="cw-pdf-button-prev"
                :class="{'inactive': (pageNum-1 == 0)}"
                @click="prevPage"
            />
            <span class="cw-pdf-title">{{block.data.pdf_title}}</span> 
            <a :href="coursewarePath + block.data.pdf_file" class="cw-pdf-download" download></a>
            <span>(Seite: <span class="cw-pdf-page-num">{{pageNum}}</span> von <span class="cw-pdf-page-count">{{pageCount}}</span>)</span>
            <button
                class="cw-pdf-button-next"
                :class="{'inactive': (pageNum == pageCount)}"
                @click="nextPage"
            />
        </div>
        <canvas 
            ref="pdfcanvas"
            class="cw-pdf-canvas"
            @mousedown="browse = true"
            @mouseup="browse = false"
            @mouseleave="browse = false"
            @mousemove="browsePdf"
        />
    </div>
    <div v-else class="PdfBlock">
        <div class="cw-pdf-header">
            <span class="cw-pdf-title">{{block.data.pdf_title}}</span> 
        </div>
        <div class="cw-pdf-downloadbox">
            <a :href="coursewarePath + block.data.pdf_file" download>
                <span class="cw-pdf-file-info cw-pdf-fileicon-pdf"> {{block.data.pdf_filename}} </span>
                <span name="download" class="cw-pdf-download-icon" ></span>
            </a>
        </div>
    </div>
</template>

<script>
import pdfjsLib from 'pdfjs-dist'

export default {
    name: 'PdfBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data() {
        return {
            PdfViewer: true,
            pdfDoc: null,
            pageNum: 1,
            pageRendering: false,
            pageNumPending: null,
            pageCount: 0,
            scale: 2,
            canvas: {},
            context: {},
            browse: false,
            browseDirection: []
        }
    },
    watch:{
        browseDirection: function(val) {
            if (val.length > 6) {
                this.evaluateBrowseAction();
            }
        }
    },
    beforeMount() {
        if (window.location.protocol == 'file:') {
            this.PdfViewer = false;
        }
    },
    mounted() {
        if (this.PdfViewer) {
            let view = this;
            this.canvas = this.$refs.pdfcanvas;
            this.context = this.canvas.getContext('2d');
            pdfjsLib.getDocument(this.coursewarePath + this.block.data.pdf_file).promise.then(function(pdf) {
                view.pdfDoc = pdf;
                view.pageCount = view.pdfDoc.numPages;
                view.renderPage(view.pageNum);
            });
        }
    },
    methods: {
        renderPage(num) {
            let view = this;
            this.pageRendering = true;
            this.pdfDoc.getPage(num).then(function(page) {
                let viewport = page.getViewport({scale: view.scale});
                view.canvas.height = viewport.height;
                view.canvas.width = viewport.width;

                let renderContext = {
                    canvasContext: view.context,
                    viewport: viewport
                };
                let renderTask = page.render(renderContext);

                renderTask.promise.then(function() {
                    view.pageRendering = false;
                    if (view.pageNumPending !== null) {
                        view.renderPage(view.pageNumPending);
                        view.pageNumPending = null;
                    }
                });
            });
        },
        queueRenderPage(num) {
            if (this.pageRendering) {
                this.pageNumPending = num;
            } else {
                this.renderPage(num);
            }
        },
        prevPage() {
            if (this.pageNum <= 1) {
                return;
            }
            this.pageNum--;
            this.queueRenderPage(this.pageNum);
        },
        nextPage () {
            if (this.pageNum >= this.pdfDoc.numPages) {
                return;
            }
            this.pageNum++;
            this.queueRenderPage(this.pageNum);
        },
        browsePdf(e){
            if (this.browse) {
                this.browseDirection.push(e.clientX);
            }
        },
        evaluateBrowseAction() {
            this.browse = false;
            let first = this.browseDirection[0];
            let last = this.browseDirection.pop();
            this.browseDirection = [];
            if (first < last) {
                this.prevPage();
            } else {
                this.nextPage();
            }
        }
    }
}
</script>

<style lang="less">
.PdfBlock {
    .cw-pdf-header {
        min-width: 10%;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        max-width: 100%;
        font-style: normal;
        font-weight: 600;
        color: #28497c;
        font-size: 16px;
        line-height: 20px;
        padding: 10px 20px;
        position: relative;
        margin: 0;
        text-align: center;
        border: solid thin #ccc;
        background-color: #fff;
        margin: 0;
        border-bottom: none;

        .cw-pdf-button-prev, 
        .cw-pdf-button-next {
            position: absolute;
            border: none;
            background-repeat: no-repeat;
            background-size: 24px;
            background-color: transparent;
            height: 24px;
            width: 24px;
            margin: 2px 12px;
            cursor: pointer;
        }

        .cw-pdf-button-prev {
            left: 0;
            background-image: url("../assets/icons/blue/arr_1left.svg");
            &.inactive {
                background-image: url("../assets/icons/lightblue/arr_1left.svg");
            }
        }

        .cw-pdf-button-next {
            right: 0;
            background-image: url("../assets/icons/blue/arr_1right.svg");
            &.inactive {
                background-image: url("../assets/icons/lightblue/arr_1right.svg");
            }
        }

        .cw-pdf-download {
            display: inline-block;
            width: 16px;
            height: 16px;
            margin: 0 0.25em;
            background: no-repeat scroll 0 0;
            background-image: url("../assets/icons/blue/download.svg");
            border: none;
            cursor: pointer;
        }
    }
    .cw-pdf-canvas {
        border: solid thin #ccc;
        width: 890px;
    }
    .cw-pdf-downloadbox {
        border: solid thin #ccc;
        padding: 0.5em 1em;

        .cw-pdf-file-info {
            background-image: url("../assets/icons/blue/file.svg");
            display: inline-block;
            background-repeat: no-repeat;
            background-size: 24px;
            padding-left: 26px;
            margin: 1em;
            line-height: 24px; 
            color: #28497c;
            &.cw-pdf-fileicon-pdf {
                background-image: url("../assets/icons/blue/file-pdf.svg");
            }
        }
        .cw-pdf-download-icon {
            float: right;
            background-image: url("../assets/icons/blue/download.svg");
            height: 24px;
            width: 24px;
            background-size: 24px;
            background-repeat: no-repeat;
            margin: 1em;
        }
    }
}
</style>
