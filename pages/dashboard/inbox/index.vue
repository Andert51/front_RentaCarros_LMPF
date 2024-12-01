<template>
  <div>
    <h1>Review Inbox</h1>
    <inbox-review />
  </div>
</template>

<script>
import inboxReview from '~/components/ui/inboxReview.vue'

export default {
  components: {
    inboxReview
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>
</style>
