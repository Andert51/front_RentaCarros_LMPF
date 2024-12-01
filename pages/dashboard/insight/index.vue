<template>
  <div>
    <h1>Sales Insight</h1>
    <salesInsight />
  </div>
</template>

<script>
import salesInsight from '~/components/ui/salesInsight.vue'
export default {
  components: {
    salesInsight
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>

</style>
