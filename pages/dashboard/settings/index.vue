<template>
  <div>
    <h1>Settings</h1>
    <settings-panel />
  </div>
</template>

<script>
import settingsPanel from '~/components/misc/settingsPanel.vue'

export default {
  components: {
    settingsPanel
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>
</style>
