<template>
  <div class="hello">
    <div v-if="!filteredQuestions.length" class="game-form">
      <h1 class="header">Welcome to Trivia Pal!</h1>
      <h3 class="header">Create a new game!</h3>
      <b-form>
          <label class="mr-sm-2" for="inline-form-custom-select-pref">Difficulty</label>
          <b-form-select
            id="input-1"
            type="email"
            placeholder="Enter email"
            v-model="selectedDifficulty"
            :options="['easy', 'medium', 'hard']"
            required
          ></b-form-select>
          <label class="mr-sm-2" for="inline-form-custom-select-pref">Category</label>
          <b-form-select
            id="input-1"
            type="email"
            v-model="selectedCategory"
            placeholder="Enter email"
            :options="['Geography', 'Politics', 'Books', 'Sports', 'History', 'Entertainment: Film', 'Mythology', 'Science & Nature','Entertainment: Video Games', 'Entertainment: Music', 'Science: Computers',]"
            required
          ></b-form-select>
          <label class="mr-sm-2" for="inline-form-custom-select-pref">Number of Questions</label>
          <b-form-select
            id="input-1"
            type="email"
            placeholder="Enter email"
            v-model="selectedNumberOfQuestions"
            :options="['5', '10']"
            required
          ></b-form-select>
              <b-button @click="filterQuestions">Start Game!</b-button>
      </b-form>
    </div>
    <div v-if="filteredQuestions.length">
      <Game
      v-for="(question, index) in filteredQuestions"
      v-bind:key="question.id"
      v-bind:question="question"
      v-bind:currentQuestion="currentQuestion"
      v-bind:nextQuestion="nextQuestion"
      v-bind:index="index"
      v-bind:questionAnsweredCorrectly="questionAnsweredCorrectly"
      v-bind:category="selectedCategory"
      v-bind:difficulty="selectedDifficulty"
      v-bind:totalQuestions="selectedNumberOfQuestions"
      >
      </Game>
    </div>
    <div>
      <Leaderboard 
      v-if="currentQuestion >= selectedNumberOfQuestions"
      v-bind:numberOfCorrectAnswers="numberOfCorrectAnswers"
      v-bind:totalQuestions="selectedNumberOfQuestions"
      v-bind:resetGame="resetGame"
      />
    </div>
  </div>
</template>

<script>
import Game from './Game.vue'
import Leaderboard from './Leaderboard.vue'
import axios from 'axios'

export default {
  name: 'Lobby',
  props: {
    msg: String
  },
  components: {
    Game,
    Leaderboard
  },
  data: function(){
    return {
      selectedNumberOfQuestions: 5,
      selectedCategory: '',
      selectedDifficulty: '',
      questions: [],
      filteredQuestions: [] ,
      currentQuestion: 0,
      numberOfCorrectAnswers: 0,
      numberOfIncorrectAnswers: 0,
    }
  },
  mounted: async function(){
      const res = await axios.get('http://localhost:3000/questions')
      try {
        this.questions = res.data
      } catch (error) {
        console.log(error)
    }
  },
  methods: {
    fetchQuestions: async function(){
      console.log(this.questions)
    },
    filterQuestions: function(){
      const category = this.questions.filter(q => q.category === this.selectedCategory)
      const difficulty = category.filter(q => q.difficulty === this.selectedDifficulty)
      const shuffled = this.shuffle(difficulty)
      
      this.filteredQuestions = shuffled.slice(0, this.selectedNumberOfQuestions)
    },
    nextQuestion: function(){
      this.currentQuestion++
    },
    questionAnsweredCorrectly: function(){
      this.numberOfCorrectAnswers++
    },
    shuffle: function(array){
      let currentIndex = array.length,  randomIndex;
      while (currentIndex != 0) {

        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;

        [array[currentIndex], array[randomIndex]] = [
        array[randomIndex], array[currentIndex]];
      }

      return array;
    },
    resetGame: function(){
      this.selectedNumberOfQuestions = 5
      this.selectedCategory = ''
      this.selectedDifficulty = ''
      this.filteredQuestions = []
      this.currentQuestion = 0
      this.numberOfCorrectAnswers = 0
    }
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.game-form {
  text-align: center;
  margin-left: 25%;
  margin-right: 25%;
  margin-top: 10%;
  font-family: Impact, Charcoal, sans-serif;
}
</style>
