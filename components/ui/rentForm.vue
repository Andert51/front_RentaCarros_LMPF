<template>
  <div>
    <!-- Rental Summary -->
    <v-card class="mb-4" outlined>
      <v-row>
        <v-col cols="4">
          <v-img :src="selectedCar?.image" aspect-ratio="1.7" />
        </v-col>
        <v-col cols="8">
          <v-card-title>{{ selectedCar?.carName || 'Car Name' }}</v-card-title>
          <v-card-subtitle>
            {{ selectedCar?.brand || 'Brand' }} - {{ selectedCar?.carModel || 'Model' }}
          </v-card-subtitle>
          <v-card-text>
            <p>Year: {{ selectedCar?.carYear || 'N/A' }}</p>
            <p>Price: ${{ selectedCar?.rentPrice || '0' }} / day</p>
            <p>Capacity: {{ selectedCar?.capacity || 'N/A' }} people</p>
          </v-card-text>
        </v-col>
      </v-row>
    </v-card>

    <!-- Rent Now Button -->
    <v-btn color="primary" @click="dialog = true">
      Rent Now
    </v-btn>

    <!-- Form Dialog -->
    <v-dialog v-model="dialog" max-width="800">
      <v-card>
        <v-card-title class="text-h5">
          Car Rental Form
        </v-card-title>
        <v-card-text>
          <v-stepper v-model="step">
            <!-- Step 1: Client Info -->
            <v-stepper-step :complete="step > 1" step="1">
              Client Info
            </v-stepper-step>
            <v-stepper-content step="1">
              <v-container>
                <v-row>
                  <v-col cols="6">
                    <v-select
                      v-model="selectedClient"
                      label="Select Client"
                      :items="clients"
                      item-text="fullname"
                      item-value="id"
                      outlined
                      required
                    />
                  </v-col>
                  <v-col cols="6">
                    <v-text-field
                      label="Phone"
                      :value="selectedClientData?.phone"
                      readonly
                      outlined
                    />
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      label="Address"
                      :value="selectedClientData?.address"
                      readonly
                      outlined
                    />
                  </v-col>
                  <v-col cols="6">
                    <v-text-field
                      label="City"
                      :value="selectedClientData?.city"
                      readonly
                      outlined
                    />
                  </v-col>
                </v-row>
                <v-btn color="primary" @click="step = 2">
                  Next
                </v-btn>
              </v-container>
            </v-stepper-content>

            <!-- Step 2: Rental Info -->
            <v-stepper-step :complete="step > 2" step="2">
              Rental Info
            </v-stepper-step>
            <v-stepper-content step="2">
              <v-container>
                <v-row>
                  <v-col cols="6">
                    <v-menu
                      ref="pickupDate"
                      v-model="pickupDateMenu"
                      :close-on-content-click="false"
                      transition="scale-transition"
                      offset-y
                      min-width="290px"
                    >
                      <template #activator="{ on, attrs }">
                        <v-text-field
                          v-bind="attrs"
                          v-model="rental.startDate"
                          label="Pick-up Date"
                          readonly
                          outlined
                          required
                          v-on="on"
                        />
                      </template>
                      <v-date-picker
                        v-model="rental.startDate"
                        @input="pickupDateMenu = false"
                      />
                    </v-menu>
                  </v-col>
                  <v-col cols="6">
                    <v-menu
                      ref="dropoffDate"
                      v-model="dropoffDateMenu"
                      :close-on-content-click="false"
                      transition="scale-transition"
                      offset-y
                      min-width="290px"
                    >
                      <template #activator="{ on, attrs }">
                        <v-text-field
                          v-bind="attrs"
                          v-model="rental.endDate"
                          label="Drop-off Date"
                          readonly
                          outlined
                          required
                          v-on="on"
                        />
                      </template>
                      <v-date-picker
                        v-model="rental.endDate"
                        @input="dropoffDateMenu = false"
                      />
                    </v-menu>
                  </v-col>
                  <v-col cols="6">
                    <v-text-field
                      v-model="rental.pickupAddress"
                      label="Pick-up Location"
                      outlined
                      required
                    />
                  </v-col>
                  <v-col cols="6">
                    <v-text-field
                      v-model="rental.dropoffAddress"
                      label="Drop-off Location"
                      outlined
                      required
                    />
                  </v-col>
                </v-row>
                <v-btn color="primary" @click="step = 3">
                  Next
                </v-btn>
                <v-btn text @click="step = 1">
                  Back
                </v-btn>
              </v-container>
            </v-stepper-content>

            <!-- Step 3: Confirmation -->
            <v-stepper-step step="3">
              Confirmation
            </v-stepper-step>
            <v-stepper-content step="3">
              <v-container>
                <v-row>
                  <v-col cols="12">
                    <p>
                      <strong>Total Amount:</strong>
                      ${{ calculateTotalAmount() }}
                    </p>
                  </v-col>
                  <v-col cols="12">
                    <v-checkbox
                      v-model="rental.agreeTerms"
                      label="I agree to the terms and conditions."
                      required
                    />
                  </v-col>
                </v-row>
                <v-btn color="success" @click="submitRental">
                  Confirm
                </v-btn>
                <v-btn text @click="step = 2">
                  Back
                </v-btn>
              </v-container>
            </v-stepper-content>
          </v-stepper>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data () {
    return {
      dialog: false,
      step: 1,
      clients: [],
      selectedClient: null,
      selectedClientData: null,
      selectedCar: null, // Data of the selected car
      rental: {
        startDate: '',
        endDate: '',
        pickupAddress: '',
        dropoffAddress: '',
        agreeTerms: false
      },
      pickupDateMenu: false,
      dropoffDateMenu: false
    }
  },
  watch: {
    selectedClient: 'fetchSelectedClient'
  },
  async mounted () {
    await this.fetchClients()
    const carId = this.$route.query.carId
    if (carId) {
      this.selectedCar = await this.$axios.get(`/car/id/${carId}`).then(res => res.data.car)
    }
  },
  methods: {
    async fetchClients () {
      const response = await this.$axios.get('/client/all')
      this.clients = response.data.clients
      // eslint-disable-next-line no-console
      console.log('Clients fetched:', this.clients)
    },
    async fetchSelectedClient () {
      if (this.selectedClient) {
        const response = await this.$axios.get(`/client/id/${this.selectedClient}`)
        this.selectedClientData = response.data.client
      }
    },
    calculateTotalAmount () {
      const days =
        (new Date(this.rental.endDate) - new Date(this.rental.startDate)) /
        (1000 * 60 * 60 * 24)
      return days * (this.selectedCar?.rentPrice || 0)
    },
    async submitRental () {
      const rentalData = {
        clientId: this.selectedClient,
        carId: this.selectedCar.id,
        startDate: this.rental.startDate,
        endDate: this.rental.endDate,
        pickupAddress: this.rental.pickupAddress,
        dropoffAddress: this.rental.dropoffAddress,
        paymentMethod: this.selectedClientData?.paymentMethod,
        totalAmount: this.calculateTotalAmount(),
        status: 'active'
      }
      await this.$axios.post('/rental/add', rentalData)
      this.dialog = false
      this.$emit('alert', 'Rental completed successfully!', 'success')
    }
  }
}
</script>
