<template>
  <div class="question">
      <div>
          <h3>{{ question.category }}</h3>
          <h3>{{ question.difficulty }}</h3>
          <h3>{{ index }}</h3>
      </div>
      <div>
          <h3>{{ question.question }}</h3>
      </div>
      <div>
          <p>{{ question.correct_answer }}</p>
          <p>{{ question.incorrect_answers[0] }}</p>
          <p>{{ question.incorrect_answers[1] }}</p>
          <p>{{ question.incorrect_answers[2] }}</p>
      </div>
  </div>
</template>

<script>
// console.log(questions)

export default {
  name: 'Game',
  props: ['question', "index"]
}
</script>

<style>

    .question{
        border: solid;
    }

</style>