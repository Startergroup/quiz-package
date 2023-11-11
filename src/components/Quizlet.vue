<template>
  <div class="quizlet">
    <div
      v-if="!hasQuizStarted && !isFinished"
      class="qz-wrapper"
    >
      <Introduction
        :title="quizlet.introductionText"
        :logoURL="quizlet.logoURL"
        @startGame="startGame"
      >
        <template #form="{ startQuizlet }">
          <slot
            name="introductionForm"
            :start-quizlet="startQuizlet"
          />
        </template>
      </Introduction>
    </div>

    <div
      v-else-if="currentQuestion && !isFinished"
      class="qz-wrapper"
    >
      <header class="quizlet__header">
        <img
          v-if="quizlet.logoURL"
          :src="quizlet.logoURL"
          alt="Logo"
          class="logo"
        >

        <Timer
          :duration="quizlet.duration"
          @getSpentTime="setSpentTime($event)"
          @timeOver="timeOver"
        />
      </header>

      <Question :value="currentQuestion.value" />

      <div class="answers">
        <Answer
          v-for="(answer, index) in currentQuestion.answers"
          :key="index"
          :answer="answer"
          @onClick="selectAnswer($event)"
        />
      </div>

      <ProgressBar
        :currentNumberQuestion="count"
        :totalNumberQuestions="getQuestions.length"
      />
    </div>

    <div v-else>
      <slot name="finalFrame">
        <p class="quizlet__finish">Спасибо за участие</p>
      </slot>
    </div>
  </div>
</template>

<script>
import Answer from './Answer.vue'
import Introduction from './Introduction.vue'
import Question from './Question.vue'
import ProgressBar from './ProgressBar.vue'
import Timer from './Timer.vue'

export default {
  name: 'Quizlet',
  props: {
    quizlet: {
      type: Object,
      required: true
    },
    isFinished: {
      type: Boolean,
      default: false
    }
  },
  components: {
    Answer,
    Introduction,
    Question,
    ProgressBar,
    Timer
  },
  data () {
    return {
      count: 0,
      hasQuizStarted: false,
      results: {
        _id: null,
        time: null,
        answers: []
      }
    }
  },
  computed: {
    getQuestions () {
      return this.quizlet.questions
    },
    currentQuestion () {
      return this.getQuestions[this.count]
    }
  },
  methods: {
    selectAnswer (answer) {
      this.updateAnswers(answer)
      this.count++

      if (this.count === this.getQuestions.length) {
        this.results._id = this.quizlet.id
        this.$emit('finishQuizlet', { results: this.results })
      }
    },
    startGame () {
      this.hasQuizStarted = true
    },
    timeOver () {
      this.results._id = this.quizlet.id
      this.results.answers = []
      this.$emit('finishQuizlet', { results: this.results })
    },
    updateAnswers (answer) {
      this.results.answers.push(answer)
    },
    setSpentTime (time) {
      this.results.time = time
    }
  }
}
</script>
