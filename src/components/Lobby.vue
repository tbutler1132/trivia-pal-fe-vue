<template>
  <div class="hello">
    Lobby
    <b-form>
        <label class="mr-sm-2" for="inline-form-custom-select-pref">Difficulty</label>
        <b-form-select
          id="input-1"
          type="email"
          placeholder="Enter email"
          v-model="selectedDifficulty"
          :options="['easy', 'Two', 'Three']"
          required
        ></b-form-select>
        <label class="mr-sm-2" for="inline-form-custom-select-pref">Category</label>
        <b-form-select
          id="input-1"
          type="email"
          v-model="selectedCategory"
          placeholder="Enter email"
          :options="['Geography', 'Politics', 'Books']"
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
    <p>{{ selectedCategory }}</p>
    <div v-if="filteredQuestions.length">
      <Game 
      v-bind:questions="filteredQuestions"
      />
    </div>
    <Leaderboard />
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
      selectedNumberOfQuestions: '',
      selectedCategory: '',
      selectedDifficulty: '',
      questions: [],
      filteredQuestions: [] ,
      currentQuestion: 1,
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
      this.filteredQuestions = difficulty
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
</style>
