<template>
  <div>
      <h1>Good game! You score was {{ numberOfCorrectAnswers }} / {{ totalQuestions }}</h1>
      <button @click="resetGame">Play again?</button>
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
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>