<template>
  <Quizlet
    :quizlet="quizlet"
    :isFinished="isFinished"
    @finishQuizlet="finishQuizlet($event)"
  >
    <template #introductionForm="{ startQuizlet }">
      <IntroductionForm :start-quizlet="startQuizlet" />
    </template>
  </Quizlet>
</template>

<script>
import IntroductionForm from '@/components/IntroductionForm.vue'
import Quizlet from '@/components/Quizlet.vue'

export default {
  name: 'App',
  components: {
    IntroductionForm,
    Quizlet
  },
  data () {
    return {
      quizlet: {
        id: 1,
        isActive: 1,
        introductionText: '1',
        duration: '02:00',
        logoURL: '',
        section: 'Секция А',
        bgColor: 'rgba(0, 0, 0, 1)',
        questions: [
          {
            value: '1',
            question_id: 1,
            quiz_id: 1,
            answers: [
              {
                answer_id: 1,
                _question_id: 1,
                value: '2',
                isRightAnswer: 1
              },
              {
                answer_id: 2,
                _question_id: 1,
                value: '3',
                isRightAnswer: 0
              },
              {
                answer_id: 3,
                _question_id: 1,
                value: '4',
                isRightAnswer: 0
              },
              {
                answer_id: 4,
                _question_id: 1,
                value: '5',
                isRightAnswer: 0
              }
            ]
          }
        ]
      },
      isFinished: false
    }
  },
  methods: {
    finishQuizlet (payload) {
      this.isFinished = true
      console.log(payload)
    }
  }
}
</script>

<style lang="scss">
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
</style>
