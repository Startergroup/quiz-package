<template>
  <div class="introduction">
    <img
      v-if="logoURL"
      :src="logoURL"
      class="introduction__logo"
      alt="logo"
    >
    <h2 class="introduction__title">{{ title }}</h2>
    <slot
      name="form"
      :start-quizlet="handleClick"
    />
  </div>
</template>

<script>
export default {
  name: 'Introduction',
  props: {
    title: {
      type: String,
      required: true
    },
    logoURL: {
      type: String,
      default: null
    }
  },
  methods: {
    async handleClick (func = () => {}) {
      await func()
      this.$emit('startGame')
    }
  }
}
</script>
