<template>
  <div v-if="questions.length">
    <b-container fluid>
    <b-row>
    <b-col>
    <div v-if="!filteredQuestions.length" class="game-form">
      <h1 class="header">Welcome to TriviaPal!</h1>
      <br/>
      <h3 class="header">Create a new game!</h3>
      <br/>
        <b-form id="game-form">
            <b-form-group
            id="input-group-1"
            label="Difficulty"
            label-for="input-1"
            form="game-form"
            >
              <b-form-select
                id="input-1"
                v-model="selectedDifficulty"
                :options="['Easy', 'Medium', 'Hard']"
                required
              ></b-form-select>
            </b-form-group>
            <b-form-group
            id="input-group-2"
            label="Category"
            label-for="input-2"
            
            >
            <b-form-select
              id="input-2"
              v-model="selectedCategory"
              :options="['Geography', 'Politics', 'Books', 'Sports', 'History', 'Entertainment: Film', 'Mythology', 'Science & Nature','Entertainment: Video Games', 'Entertainment: Music', 'Science: Computers',]"
              required
            ></b-form-select>
            </b-form-group>
            <b-form-group
            id="input-group-3"
            label="Number of Questions"
            label-for="input-3" 
            >
            <b-form-select
              id="input-3"
              v-model="selectedNumberOfQuestions"
              :options="['5', '10']"
              required
            ></b-form-select>
            </b-form-group>
            <b-button @click="filterQuestions">Start Game!</b-button>
        </b-form>
    </div>
    </b-col>
    </b-row>
    </b-container>
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
      v-bind:numberOfCorrectAnswers="numberOfCorrectAnswers"
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
  <div v-else>
    Loading...
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
      selectedCategory: 'Geography',
      selectedDifficulty: 'Easy',
      questions: [],
      filteredQuestions: [] ,
      currentQuestion: 0,
      numberOfCorrectAnswers: 0,
      numberOfIncorrectAnswers: 0,
    }
  },
  mounted: async function(){
    const res = await axios.get('https://dry-cove-27523.herokuapp.com/questions')
      try {
        this.questions = res.data
      } catch (error) {
        console.log(error)
    }
        console.log("QUESTIONS", this.questions)
  },
  methods: {
    filterQuestions: function(){
      const category = this.questions.filter(q => q.category === this.selectedCategory)
      const difficulty = category.filter(q => q.difficulty === this.selectedDifficulty.toLowerCase())
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
      this.selectedCategory = 'Geography'
      this.selectedDifficulty = 'Easy'
      this.filteredQuestions = []
      this.currentQuestion = 0
      this.numberOfCorrectAnswers = 0
    }
  }
}
</script>

<style>
.header {
  text-align: center;
}
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
  margin: auto;
  margin-top: 10%;
  font-family: Impact, Charcoal, sans-serif;
}
.create-game-form {
  border: solid;
  height: 250px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
} 
#input-1{
  width: 18%;
  margin-bottom: 15px;
}
#input-2{
  width: 18%;
  margin-bottom: 15px;
}
#input-3{
  width: 18%;
  margin-bottom: 15px;
}
</style>
