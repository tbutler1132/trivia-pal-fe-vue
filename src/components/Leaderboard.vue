<template>
  <div >
      <h1>Good game! Your final score was {{ numberOfCorrectAnswers }} / {{ totalQuestions }}</h1>
      <b-button @click="resetGame">Play again?</b-button>
  </div>
</template>

<script>


export default {
  name: 'Leaderboard',
  props: ['totalQuestions', 'numberOfCorrectAnswers', 'resetGame']
}
</script>

<style>
#app {
  font-family: Impact, Charcoal, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>