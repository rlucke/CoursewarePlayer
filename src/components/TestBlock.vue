<template>
  <div class="TestBlock">
    <div class="exercisetitle">
      <button name="exercisenav" class="exercisenavbutton exercisenavback" @click="navBack"/>
      Frage {{currentQuestion}} / {{block.data.exCount}}
      <button name="exercisenav" class="exercisenavbutton exercisenavnext" @click="navNext"/>
    </div>
    <form class="exercise" v-for="exercise in block.data.exercises" :key="exercise.id" :name="'exercise'+exercise.exercise_index" ref="exercises">
      <div class="cw-test-content" v-show="exercise.exercise_index == currentQuestion">
        <h4>{{exercise.title}}</h4>
        <div class="question-description" v-html="exercise.question_description">
        </div>
        <div class="question" v-html="exercise.question">
        </div>
        <button @click.prevent="getFormData">get form data</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  // Block is not ready yet! Do not use it!
    name: 'TestBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data() {
      return{
        currentQuestion: 1
      }
    },
    methods: {
      navBack() {
        if (this.currentQuestion > 1) {
          this.currentQuestion = this.currentQuestion -1;
        } else {
          this.currentQuestion = this.block.data.exCount;
        }
      },
      navNext(){
        if (this.currentQuestion == this.block.data.exCount) {
          this.currentQuestion = 1;
        } else {
          this.currentQuestion = this.currentQuestion + 1;
        }
      },
      getFormData(){
        let form = this.$refs.exercises[this.currentQuestion-1];
        console.log(form);
        let formData = this.serialize(form);
        console.log(formData);
      },
      serialize(form) {
        let field;
        let r =  {};

        if (typeof form == 'object' && form.nodeName == "FORM") {
          var len = form.elements.length;

          for (var i = 0; i < len; i++) {
            field = form.elements[i];
            if (field.name && !field.disabled && field.type != 'button' && field.type != 'file' && field.type != 'hidden' && field.type != 'reset' && field.type != 'submit') {
              if ((field.type != 'checkbox' && field.type != 'radio') || field.checked) {
                r[field.name] = field.value;
              }
            }
          }
        }
        return r;
      }
    }
}
</script>

<style lang="less">
.TestBlock {
  border: solid thin #ccc;
  .exercisetitle{
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
    background-color: #fcfcfc;
    text-transform: uppercase;
    border: none;
    border-bottom: solid thin #ccc;
    .exercisenavbutton{
      position: absolute;
      border: none;
      height: 20px;
      width: 20px;
      margin: 2px 12px;
      cursor: pointer;

      &.exercisenavback {
          left: 0;
          background: no-repeat scroll 0 0;
          background-image: url("../assets/icons/blue/arr_1left.svg");
      }

      &.exercisenavnext {
          right: 0;
          background: no-repeat scroll 0 0;
          background-image: url("../assets/icons/blue/arr_1right.svg");
      }
    }
  }
}
</style>
