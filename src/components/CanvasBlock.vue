<template>
  <div class="CanvasBlock">
    <h1 class="cw-canvasblock-description">
        {{block.data.description}}
    </h1>
    <div class="cw-canvasblock-toolbar">
        <div class="cw-canvasblock-buttonset">
            <button class="cw-canvasblock-reset" title="Zurücksetzen" @click="reset"></button>
            <button class="cw-canvasblock-undo" title="Rückgängig" @click="undo"></button>
            <a :href="downloadImage" download="cw-img.jpeg">
                <button class="cw-canvasblock-download" title="Download"></button>
            </a>
        </div>
        <div class="cw-canvasblock-buttonset">
            <button
                v-for="(rgba, color) in colors"
                :key="color"
                class="cw-canvasblock-color"
                :class="[currentColor == color ? 'selected-color' : '', color]"
                @click="setColor(color)"
            />
        </div>
        <div class="cw-canvasblock-buttonset">
            <button
                class="cw-canvasblock-size cw-canvasblock-size-small"
                :class="{'selected-size': (currentSize == 2)}"
                title="klein"
                @click="setSize('small')"
            />
            <button
                class="cw-canvasblock-size cw-canvasblock-size-normal"
                :class="{'selected-size': (currentSize == 5)}"
                title="normal"
                @click="setSize('normal')"
            />
            <button
                class="cw-canvasblock-size cw-canvasblock-size-large"
                :class="{'selected-size': (currentSize == 8)}"
                title="groß"
                @click="setSize('large')"
            />
            <button class="cw-canvasblock-size cw-canvasblock-size-huge"
                :class="{'selected-size': (currentSize == 12)}"
                title="riesig"
                @click="setSize('huge')"
            />
        </div>
        <div class="cw-canvasblock-buttonset">
            <button 
                class="cw-canvasblock-tool cw-canvasblock-tool-pen"
                :class="{'selected-tool':(currentTool == 'pen')}"
                title="Zeichenwerkzeug"
                @click="setTool('pen')"
            />
            <button
                class="cw-canvasblock-tool cw-canvasblock-tool-text"
                :class="{'selected-tool':(currentTool == 'text')}"
                title="Textwerkzeug"
                @click="setTool('text')"
            >T</button>
        </div>
    </div>
    <img :src="imagePath" class="cw-canvasblock-original-img" ref="image">
    <input
        v-show="textInput"
        class="cw-canvasblock-text-input"
        ref="textInputField"
        @keyup="textInputKeyUp"
    />
    <canvas
        class="cw-canvasblock-canvas"
        :class="{'cw-canvasblock-tool-selected-pen': (currentTool == 'pen'), 'cw-canvasblock-tool-selected-text': (currentTool == 'text')}"
        ref="canvas"
        @mousedown="mouseDown"
        @mousemove="mouseMove"
        @mouseup="mouseUp"
        @mouseout="mouseUp"
        @mouseleave="mouseUp"
    />
    <div class="cw-canvasblock-hints">
        <div v-show="write" class="messagebox messagebox_info cw-canvasblock-text-info" >
            Texteingabe mit Enter-Taste bestätigen
        </div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'CanvasBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data() {
        return {
            context: {},
            paint: false,
            write: false,
            clickX: [],
            clickY: [],
            clickDrag: [],
            clickColor: [],
            colors: {
                'white': 'rgba(255,255,255,1)',
                'blue': 'rgba(52,152,219,1)',
                'green': 'rgba(46,204,113,1)',
                'purple': 'rgba(155,89,182,1)',
                'red': 'rgba(231,76,60,1)',
                'yellow': 'rgba(254,211,48,1)',
                'orange': 'rgba(243,156,18,1)',
                'grey': 'rgba(149,165,166,1)',
                'darkgrey': 'rgba(52,73,94,1)',
                'black': 'rgba(0,0,0,1)'
            },
            currentColor: '',
            currentColorRGBA: '',
            sizes: {'small': 2, 'normal': 5, 'large': 8, 'huge': 12},
            clickSize: [],
            currentSize: '',
            tools : {'pen': 'pen', 'text': 'text'},
            currentTool: '',
            clickTool: [],
            Text: [],
            downloadImage: new Image(),
            textInput: false
        }
    },
    computed: {
        imagePath() {
            if(this.block.data.source == 'cw') {
                return this.coursewarePath + this.block.data.url;
            } else {
                return this.block.data.url
            }
        }
    },
    mounted() {
        this.buildCanvas(this.$refs.image);
    },
    methods: {
        setColor(color) {
            if (this.write) {
                return;
            }
            this.currentColor = color;
            this.currentColorRGBA = this.colors[color];
        },
        setSize(size) {
            if (this.textInput) {
                return;
            }
            this.currentSize = this.sizes[size];
        },
        setTool(tool) {
            if (this.write) {
                this.clickX.pop();
                this.clickY.pop();
                this.clickDrag.pop();
                this.clickColor.pop();
                this.clickSize.pop();
                this.clickTool.pop();
                this.write = false;
                this.textInput = false;
            }
            this.currentTool = this.tools[tool];
        },
        reset() {
            this.clickX.length = 0;
            this.clickY.length = 0;
            this.clickDrag.length = 0;
            this.clickColor.length = 0;
            this.clickSize.length = 0;
            this.clickTool.length = 0;
            this.Text.length = 0;
            this.paint = false;
            this.write = false;
            this.textInput = false;
            this.redraw();
        },
        buildCanvas(image) {
            let canvas = this.$refs.canvas;
            canvas.width = 889;
            if (image.height > 0) {
                canvas.height = Math.round((canvas.width / image.width) * image.height);
            } else {
                canvas.height = 500;
            }
            this.context = canvas.getContext( '2d' );
            this.currentColor = 'blue';
            this.currentColorRGBA = this.colors['blue'];
            this.currentSize = this.sizes['normal'];
            this.currentTool = this.tools['pen'];
            this.redraw();
        },
        redraw() {
            let view = this;
            let outlineImage = new Image();

            outlineImage.onload = function() {
                let context = view.context;
                let clickX = view.clickX;
                let clickY = view.clickY;
                context.clearRect(0, 0, context.canvas.width, context.canvas.height); // Clears the canvas
                context.fillStyle = "#ffffff";
                context.fillRect(0, 0, context.canvas.width, context.canvas.height); // set background
                context.drawImage(outlineImage, 0, 0, context.canvas.width, context.canvas.height);
                context.lineJoin = "round";
                for(var i=0; i < clickX.length; i++) {
                    if (view.clickTool[i] == 'pen') {
                        context.beginPath();
                        if(view.clickDrag[i] && i) {
                            context.moveTo(clickX[i-1], clickY[i-1]);
                        } else {
                            context.moveTo(clickX[i]-1, clickY[i]);
                        }
                        context.lineTo(clickX[i], clickY[i]);
                        context.closePath();
                        context.strokeStyle = view.clickColor[i];
                        context.lineWidth = view.clickSize[i];
                        context.stroke();
                    }
                    if (view.clickTool[i] == 'text') {
                        let fontsize = view.clickSize[i] * 6;
                        context.font = fontsize+"px Arial ";
                        context.fillStyle = view.clickColor[i];
                        context.fillText(view.Text[i], clickX[i], clickY[i]+fontsize);
                    }
                }
                view.downloadImage = view.context.canvas.toDataURL('image/jpeg', 1.0);
            }
            outlineImage.src = this.$refs.image.src;
            //TODO no image canvas
        },
        mouseDown(e) {
            if (this.write) {
                let view = this;
                this.$refs.textInputField.focus();
                window.setTimeout(function () {
                    view.$refs.textInputField.focus();
                }, 0);
                return;
            }
            if(this.currentTool == 'pen') {
                this.paint = true;
                this.addClick(e.offsetX, e.offsetY, false);
                this.redraw();
            }
            if(this.currentTool == 'text') {
                this.write = true;
                this.addClick(e.offsetX, e.offsetY, false);
            }
        },
        mouseMove(e) {
            if(this.paint){
                this.addClick(e.offsetX, e.offsetY, true);
                this.redraw();
            }
        },
        mouseUp(e) {
            this.paint = false;
        },
        addClick(x, y, dragging) {
            this.clickX.push(x);
            this.clickY.push(y);
            this.clickDrag.push(dragging);
            this.clickColor.push(this.currentColorRGBA);
            this.clickSize.push(this.currentSize);
            this.clickTool.push(this.currentTool);
            if (this.currentTool == 'text') {
                this.enableTextInput(x, y);
            } else {
                this.Text.push('');
            }
        },
        undo() {
            let dragging = this.clickDrag[this.clickDrag.length -1];
            this.clickX.pop();
            this.clickY.pop();
            this.clickDrag.pop();
            this.clickColor.pop();
            this.clickSize.pop();
            this.clickTool.pop();
            if (this.write) {
                this.textInput = false;
                this.write = false;
            } else {
                this.Text.pop('');
            }
            if (dragging){
                this.undo();
            }
            this.redraw();
        },
        enableTextInput(x, y) {
            let view = this;
            let fontsize = this.currentSize * 6;
            this.textInput = true;
            let input = this.$refs.textInputField;
            input.value = '';
            input.style.position = 'absolute';
            input.style.top = (this.$refs.canvas.offsetTop + y) + 'px';
            input.style.left=  (320 + x) +'px';
            input.style.lineHeight = fontsize +'px';
            input.style.fontSize = fontsize +'px';
            input.style.width = '300px';
            window.setTimeout(function () {
                view.$refs.textInputField.focus();
            }, 0);
        },
        textInputKeyUp(e) {
            if (e.defaultPrevented) {
                return;
            }
            let key = e.key || e.keyCode;
            if (key === 'Enter' || key === 13) {
                this.Text.push(this.$refs.textInputField.value);
                this.textInput = false;
                this.write = false;
                this.redraw();
            }
            if (key === 'Escape' || key === 'Esc' || key === 27) {
                this.clickX.pop();
                this.clickY.pop();
                this.clickDrag.pop();
                this.clickColor.pop();
                this.clickSize.pop();
                this.clickTool.pop();
                this.textInput = false;
                this.write = false;
            }
        }
    }
}
</script>

<style lang="less">
.CanvasBlock {

    .cw-canvasblock-canvas {
        max-width: 100%;
        border: solid thin #ccc;
    }

    .cw-canvasblock-upload-message{
        display: none;
    }

    .cw-canvasblock-original-img {
        display: none;
    }

    .cw-canvasblock-tool-selected-text {
        cursor: text;
    }

    h1.cw-canvasblock-description {
        border-bottom: none;
    }

    .cw-canvasblock-toolbar {
        border: solid thin #ccc;
        border-bottom: none;
    }

    .cw-canvasblock-buttonset {
        display: inline-block;
        padding: 5px;
        margin-right: 0.5em;
    }

    .cw-canvasblock-tool-selected-pen {
        cursor: url("../assets/cursor/pencil.png") 0 32, auto;
    }
    .cw-canvasblock-tool-selected-text {
        cursor: text;
    }

    button {
        cursor: pointer;
        user-select: none;
        border: solid thin #ccc;
        height: 32px;
        width: 32px;
        background-color: white;
        background-position: center;
        background-repeat: no-repeat;
        background-size: 24px 24px;

        &:focus {
            outline: none;
        }

        &.cw-canvasblock-color {
            &.white {background-color: rgba(255,255,255,1)}
            &.blue {background-color:rgba(52,152,219,1)}
            &.green {background-color: rgba(46,204,113,1)}
            &.purple {background-color: rgba(155,89,182,1)}
            &.red {background-color: rgba(231,76,60,1)}
            &.yellow {background-color: rgba(254,211,48,1)}
            &.orange {background-color: rgba(243,156,18,1)}
            &.grey {background-color: rgba(149,165,166,1)}
            &.darkgrey {background-color: rgba(52,73,94,1)}
            &.black {background-color: rgba(0,0,0,1)}
            &.selected-color {
                border: solid 2px #000;
            }
        }

        &.cw-canvasblock-reset {
            background-image: url("../assets/icons/blue/refresh.svg");
        }

        &.cw-canvasblock-size {
            background-image: url("../assets/icons/blue/stop.svg");
            
            &.cw-canvasblock-size-small {
                background-size: 8px 7px;
            }
            &.cw-canvasblock-size-normal {
                background-size: 16px 14px;
            }
            &.cw-canvasblock-size-large {
                background-size: 22px 20px;
            }
            &.cw-canvasblock-size-huge {
                background-size: 26px 24px;
            }
            &.selected-size {
                border: solid 2px #000;
            }
        }

        &.cw-canvasblock-tool {
            &.cw-canvasblock-tool-pen {
                background-image: url("../assets/icons/blue/comment.svg");
            }

            &.cw-canvasblock-tool-text {
                vertical-align: top;
                font-size: 22px;
                color: #28497c;
                font-weight: 600;
            }

            &.selected-tool {
                border: solid 2px #000;
            }
        }

        &.cw-canvasblock-undo {
            background-image: url("../assets/icons/blue/arr_2left.svg");
        }

        &.cw-canvasblock-download {
            background-image: url("../assets/icons/blue/download.svg");
        }
        &.cw-canvasblock-store {
            background-image: url("../assets/icons/blue/upload.svg");
        }
        &.cw-canvasblock-show-all {
            background-image: url("../assets/icons/blue/group2.svg");
            &.selected-view {
                border: solid 2px #000;
            }
        }
        &.cw-canvasblock-show-own {
            background-image: url("../assets/icons/blue/headache.svg");
            &.selected-view {
                border: solid 2px #000;
            }
        }
    }

}
</style>