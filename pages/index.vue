<template>
  <login-form />
</template>

<script>
import loginForm from '~/components/ui/loginForm.vue'
export default {
  components: {
    loginForm
  },
  layout: 'login',
  middleware: 'auth'

}
</script>

<style scoped>

</style>
