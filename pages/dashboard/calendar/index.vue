<template>
  <div>
    <h1>Calendar</h1>
    <calendar-insight />
  </div>
</template>

<script>
import calendarInsight from '~/components/misc/calendarInsight.vue'

export default {
  components: {
    calendarInsight
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>
</style>
