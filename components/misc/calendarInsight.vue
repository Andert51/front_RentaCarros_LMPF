<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-card-title>
            Rental Calendar
          </v-card-title>
          <v-card-text>
            <v-calendar
              v-model="focus"
              :events="events"
              color="primary"
              event-color="blue"
              type="month"
            >
              <template #event="{ event }">
                <v-chip
                  :color="event.color"
                  dark
                  small
                >
                  {{ event.name }}
                </v-chip>
              </template>
            </v-calendar>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      focus: '',
      events: []
    }
  },
  async mounted () {
    await this.fetchRentals()
  },
  methods: {
    async fetchRentals () {
      const response = await this.$axios.get('/rental/all')
      const rentals = response.data.rentals
      this.events = rentals.map(rental => ({
        name: `${rental.clientName} - ${rental.carName}`,
        start: rental.startDate,
        end: rental.endDate,
        color: 'blue'
      }))
    }
  }
}
</script>

<style scoped>
</style>
