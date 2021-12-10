<template>
  <div class="question">
      <div>
          <h3 @click="randomizeAnswerOrder">{{ questions[currentQuestion].category }}</h3>
          <h3>{{ questions[currentQuestion].difficulty }}</h3>
          <h3>{{ index }}</h3>
      </div>
      <div>
          <h3>{{ questions[currentQuestion].question }}</h3>
      </div>
      <div>
          <p v-bind:key="question.answer" v-for="question in orderQuestions">{{ question.answer }}</p>
      </div>
  </div>
</template>

<script>
// console.log(questions)

export default {
  name: 'Game',
  props: ['questions', "index"],
  data: function(){
      return {
          currentQuestion: 0,
          orderedQuestions: [],
      }
  },
  created: function(){
      return this.randomizeAnswerOrder()
  },
  methods: {
      incrementQuestion: function(ans){
          console.log("Ans", ans)
          this.currentQuestion++
      },
      randomizeAnswerOrder: function(){
          this.currentQuestion++
          const answerOptions = []
          this.questions[this.currentQuestion].incorrect_answers.forEach(el => {
              let obj = {
                  answer: el,
                  correct: false
              }
              answerOptions.push(obj)
          })
          answerOptions.push({answer: this.questions[this.currentQuestion].correct_answer, correct: true})
            console.log(answerOptions)
            let currentIndex = answerOptions.length,  randomIndex;
            while (currentIndex != 0) {

                // Pick a remaining element...
                randomIndex = Math.floor(Math.random() * currentIndex);
                currentIndex--;

                // And swap it with the current element.
                [answerOptions[currentIndex], answerOptions[randomIndex]] = [
                answerOptions[randomIndex], answerOptions[currentIndex]];
            }
            this.orderedQuestions = answerOptions
      }
      
  },
}
</script>

<style>

    .question{
        border: solid;
    }

</style>