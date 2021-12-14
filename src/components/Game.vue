<template>
    <div v-if="currentQuestion === index">
        <b-container fluid>
            <b-row class="header-container">
                <b-col>
                    <h3 class="header-container-text">Category: {{ category }}</h3>
                </b-col>
                <b-col>
                    <h3 class="header-container-text">Difficulty: {{ difficulty }}</h3>
                </b-col>
                <b-col>
                    <h3 class="header-container-text">Questions Left: {{ totalQuestions - index }}</h3>
                </b-col>
                <b-col>
                    <h3 class="header-container-text">Current Score: {{ numberOfCorrectAnswers }} / {{ totalQuestions }}</h3>
                </b-col>
            </b-row>
            <div>
                <b-row>
                    <b-col class="container-4" sm="7" id="game">
                            <div class="container-1" id="question-div">
                                <h1>{{ replaceCharacters(question.question) }}</h1>
                            <b-container class="container-1">
                                <div>
                                    <b-row>
                                        <b-button 
                                        id="_blank"
                                        v-for="answer in orderedAnswers"
                                        v-bind:key="answer.answer"
                                        v-on:click="determineCorrect(answer.correct, answer.answer)"
                                        v-bind:variant="answer.answer === clickedAnswer ? variant : 'light'"
                                        size="lg"
                                        v-bind:disabled="buttonsDisabled"
                                        >
                                        <span>
                                            {{ replaceCharacters(answer.answer) }}
                                        </span>
                                        </b-button>
                                    </b-row>
                                </div>
                            </b-container>
                            </div>
                    </b-col>
                    <div>
                    </div>
                </b-row>
            </div>
        </b-container>
    </div>
</template>

<script>

export default {
  name: 'Game',
  props: ['question', "index", "currentQuestion", "nextQuestion", "questionAnsweredCorrectly", "category", "difficulty", "totalQuestions", "numberOfCorrectAnswers"],
  data: function(){
      return {
          orderedAnswers: [],
          variant: "light",
          buttonsDisabled: false,
          clickedAnswer: ""
      }
  },
  created: function(){
      this.randomizeAnswerOrder()
  },
  methods: {
      incrementQuestion: function(){
          this.nextQuestion()
      },
      replaceCharacters: function(str){
          return str.replace(/&quot;|&#039;/gi, "'").replace(/&amp;/gi, "&")
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
      determineCorrect: function(correct, answer){
          this.buttonsDisabled = true
          this.clickedAnswer = answer
          if(!correct){
            this.variant = "danger"  
          }else{
            this.variant = "success"
          }
        setTimeout(() => {
            if(correct){
              this.questionAnsweredCorrectly()
            }
              this.incrementQuestion()
              this.answerButtonColor = ""
              this.clickedAnswer = ""
              this.buttonsDisabled = false
        }, 2000);
      }
      
  },
}
</script>

<style>
    .header-container {
    border: 1px #ccc solid;
    background-color: #d1faff;
    font-family: Impact, Charcoal, sans-serif;
    margin-bottom: 1%;
    }
    .header-container-text {
    margin-top: 2%;
    }
    .container-4 {
    margin-top: 1%;
    margin: auto;
    border: solid;
    border-color: white;
    background-color: #57a773;
    font-family: Impact, Charcoal, sans-serif;
    justify-content: center;
    text-align: center;
    height: 500px;
    }
    .container-1 {
    background-color: #9bd1e5;
    margin-top: 2%;
    margin-bottom: 5%;
    font-family: Impact, Charcoal, sans-serif;
    text-align: center;
    }

    #question-div {
    border: 1px #ccc solid;
    border-color: black;
    font-family: Impact, Charcoal, sans-serif;
    height: 100%;
    max-height: 450px;
    }

    [id="_blank"] {
        color: black;
        font-family: Impact, Charcoal, sans-serif;
        display: flex;
        margin-bottom: 2%;
        margin-top: 3%;
        align-items: center;
        justify-content: center;
        margin-left: auto;
        margin-right: auto;
        padding: 10px 24px;
        border-radius: 12px;
        transition: all 0.5s;
        cursor: pointer;
    }

    [id="_blank"] span {
        cursor: pointer;
        display: inline-block;
        position: relative;
        transition: 0.5s;
    }

    [id="_blank"] span:after {
        content: "\00bb";
        position: absolute;
        opacity: 0;
        top: 0;
        right: -20px;
        transition: 0.5s;
    }

    [id="_blank"]:hover span {
        padding-right: 25px;
    }

    [id="_blank"]:hover span:after {
        opacity: 1;
        right: 0;
    }
</style>