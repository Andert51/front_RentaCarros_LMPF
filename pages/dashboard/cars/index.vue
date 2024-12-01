<template>
  <div>
    <h1>Nuestros Autos Disponibles</h1>
    <carsCrud />
  </div>
</template>

<script>
import carsCrud from '~/components/ui/carsCrud.vue'
export default {
  components: {
    carsCrud
  },
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>

</style>
