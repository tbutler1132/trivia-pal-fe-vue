<template>
  <div v-if="currentQuestion === index" class="question">
      <div>
          <h3>{{ question.question }}</h3>
          <button 
          v-for="answer in orderedAnswers"
          v-bind:key="answer"
          v-on:click="determineCorrect(answer.correct)"
          >
          {{ answer.answer }}
          </button>
          <h3>{{ index }}</h3>
      </div>
      <div>
      </div>
  </div>
</template>

<script>

export default {
  name: 'Game',
  props: ['question', "index", "currentQuestion", "nextQuestion", "questionAnsweredCorrectly"],
  data: function(){
      return {
          orderedAnswers: [],
      }
  },
  created: function(){
      this.randomizeAnswerOrder()
  },
  methods: {
      incrementQuestion: function(){
          this.nextQuestion()
      },
      randomizeAnswerOrder: function(){
          const answerOptions = []
          this.question.incorrect_answers.forEach(el => {
              let obj = {
                  answer: el,
                  correct: false
              }
              answerOptions.push(obj)
          })
          answerOptions.push({answer: this.question.correct_answer, correct: true})
            let currentIndex = answerOptions.length,  randomIndex;
            while (currentIndex != 0) {

                randomIndex = Math.floor(Math.random() * currentIndex);
                currentIndex--;

                [answerOptions[currentIndex], answerOptions[randomIndex]] = [
                answerOptions[randomIndex], answerOptions[currentIndex]];
            }
            this.orderedAnswers = answerOptions
      },
      determineCorrect: function(correct){
          if(correct){
              this.questionAnsweredCorrectly()
          }
              this.incrementQuestion()
      }
      
  },
}
</script>

<style>

    .question{
        border: solid;
    }

</style>