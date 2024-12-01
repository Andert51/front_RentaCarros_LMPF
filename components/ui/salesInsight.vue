<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-select
          v-model="selectedClient"
          :items="clients"
          item-text="fullname"
          item-value="id"
          label="Select Client"
          outlined
          @change="fetchRentals"
        />
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-data-table
          :headers="headers"
          :items="rentals"
          item-key="id"
          class="elevation-1"
        >
          <template #[`item.actions`]="{ item }">
            <v-btn icon @click="editRental(item)">
              <v-icon>mdi-pencil</v-icon>
            </v-btn>
            <v-btn icon @click="deleteRental(item.id)">
              <v-icon color="red">
                mdi-delete
              </v-icon>
            </v-btn>
          </template>
        </v-data-table>
      </v-col>
    </v-row>

    <!-- Edit Rental Dialog -->
    <v-dialog v-model="dialog" max-width="600">
      <v-card>
        <v-card-title>Edit Rental</v-card-title>
        <v-card-text>
          <v-form>
            <v-row>
              <v-col cols="6">
                <v-text-field
                  v-model="selectedRental.startDate"
                  label="Start Date"
                  outlined
                  required
                />
              </v-col>
              <v-col cols="6">
                <v-text-field
                  v-model="selectedRental.endDate"
                  label="End Date"
                  outlined
                  required
                />
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="selectedRental.pickupAddress"
                  label="Pick-up Address"
                  outlined
                  required
                />
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="selectedRental.dropoffAddress"
                  label="Drop-off Address"
                  outlined
                  required
                />
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-btn color="blue darken-1" text @click="dialog = false">
            Cancel
          </v-btn>
          <v-btn color="blue darken-1" text @click="updateRental">
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      clients: [],
      rentals: [],
      selectedClient: null,
      selectedRental: {},
      dialog: false,
      headers: [
        { text: 'Car ID', value: 'carId' },
        { text: 'Start Date', value: 'startDate' },
        { text: 'End Date', value: 'endDate' },
        { text: 'Pick-up Address', value: 'pickupAddress' },
        { text: 'Drop-off Address', value: 'dropoffAddress' },
        { text: 'Total Amount', value: 'totalAmount' },
        { text: 'Status', value: 'status' },
        { text: 'Actions', value: 'actions', sortable: false }
      ]
    }
  },
  async mounted () {
    await this.fetchClients()
  },
  methods: {
    async fetchClients () {
      const response = await this.$axios.get('/client/all')
      this.clients = response.data.clients
      // eslint-disable-next-line no-console
      console.log('Clients fetched:', response)
    },
    async fetchRentals () {
      if (this.selectedClient) {
        const response = await this.$axios.get(`/rental/client/${this.selectedClient}`)
        this.rentals = response.data.rentals
        // eslint-disable-next-line no-console
        console.log('Rents fetched:', response)
      }
    },
    editRental (rental) {
      this.selectedRental = { ...rental }
      this.dialog = true
    },
    async updateRental () {
      const response = await this.$axios.put(`/rental/update/${this.selectedRental.id}`, this.selectedRental)
      if (response.data.success) {
        this.dialog = false
        await this.fetchRentals()
      }
    },
    async deleteRental (rentalId) {
      const response = await this.$axios.delete(`/rental/delete/${rentalId}`)
      if (response.data.success) {
        await this.fetchRentals()
      }
    }
  }
}
</script>

<style scoped>
</style>
