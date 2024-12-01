<template>
  <div>
    <h1>RentForm</h1>
    <rentForm />
  </div>
</template>

<script>
import rentForm from '~/components/ui/rentForm.vue'
export default {
  components: {
    rentForm
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>

</style>
