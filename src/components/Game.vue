<template>
    <div v-if="currentQuestion === index">
        <div class="game-info">
            <h3>{{ category }}</h3>
            <h3>{{ difficulty }}</h3>
            <h3>{{ totalQuestions - index }} questions left!</h3>
        </div>
        <div class="question">
            <div>
                <h3>{{ question.question }}</h3>
                <div class="answer-options">
                    <b-button 
                    v-for="answer in orderedAnswers"
                    v-bind:key="answer.answer"
                    v-on:click="determineCorrect(answer.correct)"
                    v-bind:variant="answer.correct ? answerButtonColor : incorrecAnswerButtonColors"
                    :block="true"
                    >
                    {{ answer.answer }}
                    </b-button>
                </div>
                <h3>{{ index }}</h3>
            </div>
            <div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
  name: 'Game',
  props: ['question', "index", "currentQuestion", "nextQuestion", "questionAnsweredCorrectly", "category", "difficulty", "totalQuestions"],
  data: function(){
      return {
          orderedAnswers: [],
          answerButtonColor: "",
          incorrecAnswerButtonColors: "",
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
            this.answerButtonColor = "success"  
          }else{
            this.answerButtonColor = "success"
            this.incorrecAnswerButtonColors = "danger"
          }
        setTimeout(() => {
            if(correct){
              this.questionAnsweredCorrectly()
            }
              this.incrementQuestion()
              this.answerButtonColor = ""
        }, 2000);
      }
      
  },
}
</script>

<style>

    .question{
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .answer-options{
        display: flex;
        flex-direction: column;
        align-items: center;
        border: solid;
        height: 200px;
    }
    .game-info{
        display: flex;
        justify-content: space-around;
        width: 50%
    }

</style>