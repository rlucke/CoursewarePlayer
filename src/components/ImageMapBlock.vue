<template>
  <div class="ImageMapBlock">
    <img :src="image_url" class="cw-image-map-original-img" ref="original_img">
    <canvas class="cw-image-map-canvas" ref="canvas"></canvas>
    <img class="cw-image-from-canvas" :src="image_from_canvas" ref="image_from_canvas" :usemap = "'#' + map_name"/>
    <map ref="map" :name="map_name">
      <area 
        v-for="area in areas"
        :key="area.id"
        :id="area.id"
        :shape="area.shape"
        :coords="area.coords"
        :title="area.title"
        :href="area.external_target"
        :target="area.link_target"
        @click="areaLink(area.link_type, area.internal_target)"
      >
    </map>
  </div>
</template>

<script>
export default {
    name: 'ImageMapBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data() {
      return{
          image_url: '',
          shapes: [],
          context: {},
          image_from_canvas: '',
          map_name: '',
          areas: [],
          darkColors: ['black', 'darkgrey', 'purple']
      }
    },
    mounted() {
      let content = this.block.data;
      this.image_url =  this.coursewarePath + content.image_id + '/' +content.image_name;
      this.shapes = content.shapes;
      this.buildCanvas(this.$refs.original_img);
    },
    methods: {

      buildCanvas(original_img) {
        let canvas = this.$refs.canvas;
        canvas.width = 860;
        if (original_img.height > 0) {
            canvas.height = Math.round((canvas.width / original_img.width) * original_img.height);
        } else {
            canvas.height = 484;
        }
        this.context = canvas.getContext( '2d' );
        this.drawScreen();
    },

    drawScreen() {
        let context = this.context;
        let view = this;
        let outlineImage = new Image();
        outlineImage.src = this.image_url;
        console.log(outlineImage);
        outlineImage.onload = function() {
            context.clearRect(0, 0, context.canvas.width, context.canvas.height); // Clears the canvas
            context.fillStyle = "#ffffff";
            context.fillRect(0, 0, context.canvas.width, context.canvas.height); // set background
            if (outlineImage.src != '') { 
                context.drawImage(outlineImage, 0, 0, context.canvas.width, context.canvas.height);
            }
            view.drawShapes();

            if (!(view.$refs.canvas.length > 0)) {
                view.image_from_canvas = view.context.canvas.toDataURL("image/jpeg", 1.0);
                view.mapImage();
            }
        };
    },

    drawShapes() {
        let context = this.context;
        let view = this;
        this.shapes.forEach( (value, key) => {
            let shape = value;
            let text = shape.data.text;
            let shape_width = 0, shape_height = 0, text_X = 0, text_Y = 0;

            context.beginPath();
            switch (shape.type) {
                case 'arc':
                    shape_width =  Math.round((2*shape.data.radius)/Math.sqrt(2))*0.85;
                    shape_height =  shape_width/0.85;
                    text_X = shape.data.centerX;
                    text_Y = shape.data.centerY - shape.data.radius*0.75;
                    context.arc(shape.data.centerX, shape.data.centerY, shape.data.radius, 0, 2 * Math.PI); // x, y, r, startAngle, endAngle ... Angle in radians!
                    context.fillStyle = shape.data.fillStyle;
                    context.fill();
                    break;
                case 'ellipse':
                    shape_width = shape.data.radiusX;
                    shape_height = shape.data.radiusY*1.75;
                    text_X = shape.data.X ;
                    text_Y = shape.data.Y - shape.data.radiusY*0.8;
                    context.ellipse(shape.data.X, shape.data.Y, shape.data.radiusX, shape.data.radiusY, 0, 0, 2 * Math.PI);
                    context.fillStyle = shape.data.fillStyle;
                    context.fill();
                    break;
                case 'rect':
                    shape_width = shape.data.width;
                    shape_height = shape.data.height;
                    text_X = shape.data.X + shape.data.width/2;
                    text_Y = shape.data.Y;
                    context.rect(shape.data.X, shape.data.Y, shape.data.width, shape.data.height);
                    context.fillStyle = shape.data.fillStyle;
                    context.fill();
                    break;
                default:

                    return;
            }

            if ((text) && (shape.data.colorName != 'transparent')) {
                text = view.fitTextToShape(context, text, shape_width);
                context.textAlign = "center"; 
                context.font = "14px Arial"
                if (view.darkColors.indexOf(shape.data.colorName) > -1) {
                    context.fillStyle = '#ffffff';
                } else { 
                    context.fillStyle = '#000000';
                }
                let lineHeight = shape_height/(text.length+1);
                text.forEach((value, key) => {
                    context.fillText(value, text_X, text_Y + lineHeight*(key+1));
                });
            }

            context.closePath();
        });
    },

    fitTextToShape(context, text, shape_width) {
        let text_width = context.measureText(text).width;
        if (text_width > shape_width) {
            text = text.split(' ');
            let line = "";
            let word = " ";
            let new_text = [];
            do{
                word = text.shift();
                if (context.measureText(word).width >= shape_width) {
                    return [''];
                }
                line = line + word + " ";
                if (context.measureText(line).width > shape_width) {
                    text.unshift(word);
                    line = line.substring(0, line.lastIndexOf(word));
                    new_text.push(line.trim());
                    line = "";
                }
            } while (text.length > 0)
            new_text.push(line.trim());
            return new_text;
        } else {
            return [text];
        }
    },

    mapImage() {
        let view = this;
        let image = this.$refs.image_from_canvas;
        // generate map name
        let map_name = "cw-image-map-"+Math.round(Math.random()*100);
        this.map_name = map_name;
        //append map
        let $map = this.$refs.map;
        // insert areas
        this.shapes.forEach((value, key) => {
            let shape = value;
            let area = {};
            area.id = 'shape-' + key;

            switch (shape.type) {
                case 'arc':
                    area.shape = 'circle';
                    area.coords = shape.data.centerX + ', ' + shape.data.centerY + ', ' + shape.data.radius;
                    break;
                case 'ellipse':
                    let coords = '';
                    let x = 0, y = 0;
                    for (let theta=0; theta < 2*Math.PI; theta+=2*Math.PI/20) {
                        x = shape.data.X + Math.round(shape.data.radiusX * Math.cos(theta));
                        y = shape.data.Y + Math.round(shape.data.radiusY * Math.sin(theta));
                        coords = coords + x + ',' + y + ',';
                    }
                    area.shape = 'poly';
                    area.coords = coords;
                    break;
                case 'rect':
                case 'text':
                    let x2 = shape.data.X+shape.data.width;
                    let y2 = shape.data.Y+shape.data.height;
                    area.shape = 'rect';
                    area.coords = shape.data.X + ', ' + shape.data.Y + ', ' + x2 + ', ' + y2;
                    break;
            }
            area.title = shape.title;
            shape.target && shape.link_type == 'external' ? area.external_target = shape.target : area.external_target = '#';
            shape.target && shape.link_type == 'internal' ? area.internal_target = shape.target : area.internal_target = '';
            shape.link_type == 'external' ? area.link_target = '_blank' : area.link_target = '_self';
            area.link_type = shape.link_type;
            view.areas.push(area);
        });
      },
      areaLink(link_type, internal_target) {
          if(link_type == 'external') {return};
          if(link_type == 'internal') {
            this.$emit('navigate', {type: internal_target.type, id: internal_target.id});
          };
          
      }

    }
}
</script>

<style lang="less">
.ImageMapBlock {
  .cw-image-map-canvas,
  .cw-image-map-original-img {
    display: none;
  }
  .cw-image-from-canvas {
    margin: 15px;
  }
}
</style>
