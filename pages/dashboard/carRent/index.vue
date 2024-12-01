<template>
  <v-container fluid>
    <v-row>
      <!-- Sidebar Filters -->
      <v-col cols="3">
        <v-card outlined>
          <v-card-title class="text-h6 font-weight-bold">
            Type
          </v-card-title>
          <v-list dense>
            <v-list-item v-for="type in types" :key="type.name" class="py-1">
              <v-checkbox v-model="type.selected" :label="`${type.name} (${type.count})`" @change="filterCars" />
            </v-list-item>
          </v-list>

          <v-divider class="my-2" />

          <v-card-title class="text-h6 font-weight-bold">
            Capacity
          </v-card-title>
          <v-list dense>
            <v-list-item v-for="capacity in capacities" :key="capacity.name" class="py-1">
              <v-checkbox v-model="capacity.selected" :label="`${capacity.name} (${capacity.count})`" @change="filterCars" />
            </v-list-item>
          </v-list>

          <v-divider class="my-2" />

          <v-card-title class="text-h6 font-weight-bold">
            Price
          </v-card-title>
          <v-slider
            v-model="price"
            max="100"
            step="10"
            ticks
            tick-size="3"
            color="primary"
            label
            append-icon="mdi-currency-usd"
            @change="filterCars"
          />
        </v-card>
      </v-col>

      <!-- Main Content -->
      <v-col cols="9">
        <v-row>
          <!-- Featured Car -->
          <v-col cols="12">
            <v-card outlined>
              <v-row>
                <v-col cols="6">
                  <v-img
                    src="/MercedesCLK.jpg"
                    max-height="200"
                    contain
                  />
                  <v-row dense>
                    <v-col v-for="(img, i) in featuredCar.images" :key="i" cols="3">
                      <v-img :src="img" contain class="border rounded" />
                    </v-col>
                  </v-row>
                </v-col>

                <v-col cols="6">
                  <v-card-title class="text-h5 font-weight-bold mb-3 ml-0">
                    {{ featuredCar.brand }} {{ featuredCar.carName }} {{ featuredCar.carModel }}
                  </v-card-title>
                  <v-rating value="4.5" color="amber" dense readonly />
                  <p class="text-body-2">
                    {{ featuredCar.carDescription }}
                  </p>
                  <v-row>
                    <v-col cols="6">
                      <v-chip class="ma-1" outlined small>
                        {{ featuredCar.type }}
                      </v-chip>
                      <v-chip class="ma-1" outlined small>
                        {{ featuredCar.capacity }}
                      </v-chip>
                      <v-chip class="ma-1" outlined small>
                        {{ featuredCar.gas }}
                      </v-chip>
                      <v-chip class="ma-1" outlined small>
                        {{ featuredCar.transmission }}
                      </v-chip>
                      <v-chip class="ma-1" outlined small>
                        {{ featuredCar.carYear }}
                      </v-chip>
                    </v-col>
                    <v-col cols="6">
                      <p class="text-h6 font-weight-bold">
                        {{ featuredCar.rentPrice }} / day
                      </p>
                      <p class="text-body-2 text--line-through">
                        <v-icon>
                          mdi-currency-usd
                        </v-icon>
                        Dolares Estadounidenses
                      </p>
                    </v-col>
                  </v-row>
                  <v-btn color="primary" block @click="navigateToRentForm(featuredCar.id)">
                    Rent Now
                  </v-btn>
                </v-col>
              </v-row>
            </v-card>
          </v-col>
        </v-row>

        <!-- Reviews -->
        <v-row>
          <v-col cols="12">
            <v-card outlined>
              <v-card-title class="text-h6 font-weight-bold">
                Reviews
              </v-card-title>
              <v-list>
                <v-list-item v-for="review in formattedReviews" :key="review.id" class="py-1">
                  <v-list-item-avatar>
                    <img src="/user01.png">
                  </v-list-item-avatar>
                  <v-list-item-content>
                    <v-list-item-title class="text-h6">
                      {{ review.clientName }}
                    </v-list-item-title>
                    <v-list-item-subtitle class="text-body-2">
                      {{ review.reviewText }}
                    </v-list-item-subtitle>
                    <v-list-item-subtitle class="text-body-2">
                      {{ review.createdAt }}
                    </v-list-item-subtitle>
                  </v-list-item-content>
                  <v-rating :value="review.rating" readonly color="amber" dense />
                </v-list-item>
              </v-list>
            </v-card>
          </v-col>
        </v-row>

        <!-- Recent Cars -->
        <v-row>
          <v-col cols="12">
            <v-card outlined>
              <v-card-title class="text-h6 font-weight-bold">
                Recent Cars
              </v-card-title>
              <v-row>
                <v-col
                  v-for="car in filteredCars"
                  :key="car.id"
                  cols="4"
                >
                  <v-card>
                    <v-img :src="car.image" aspect-ratio="16/9" />
                    <v-card-title class="text-h6 font-weight-bold">
                      {{ car.brand }} {{ car.type }}
                    </v-card-title>
                    <v-card-subtitle class="text-body-2">
                      {{ car.rentPrice }} / day
                    </v-card-subtitle>
                    <v-btn color="primary" block small @click="navigateToRentForm(car.id)">
                      Rent Now
                    </v-btn>
                  </v-card>
                </v-col>
              </v-row>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  layout: 'dashboard',
  middleware: 'auth',
  data () {
    return {
      // Filters
      types: [],
      capacities: [],
      price: 100,

      // Featured Car
      featuredCar: [],

      // Reviews
      reviews: [],
      clients: [],

      // Recent Cars
      recentCars: [],

      // State
      cars: [],
      filteredCars: []
    }
  },
  computed: {
    formattedReviews () {
      return this.reviews.map((review) => {
        const client = this.clients.find(client => client.id === review.clientId)
        return {
          ...review,
          clientName: client ? client.fullname : 'Unknown',
          createdAt: new Date(review.createdAt).toLocaleDateString()
        }
      })
    }
  },
  mounted () {
    this.fetchData()
    this.fetchReviews()
    this.fetchClients()
  },
  methods: {
    async fetchData () {
      try {
        // Fetch cars
        const res = await this.$axios.get('/car/all')
        if (res && res.data && res.data.success) {
          this.cars = res.data.cars
          this.filteredCars = res.data.cars
          // eslint-disable-next-line no-console
          console.log('Cars fetched:', this.cars)

          // eslint-disable-next-line no-console
          console.log('Car types:', this.cars.map(car => car.type))
          // eslint-disable-next-line no-console
          console.log('Car capacities:', this.cars.map(car => car.capacity))

          // Set filters dynamically
          this.types = [...new Set(this.cars.map(car => this.normalizeString(car.type)))].map(type => ({
            name: type,
            selected: false,
            count: this.cars.filter(car => this.normalizeString(car.type) === type).length
          }))

          this.capacities = [...new Set(this.cars.map(car => this.normalizeString(car.capacity)))].map(capacity => ({
            name: capacity,
            selected: false,
            count: this.cars.filter(car => this.normalizeString(car.capacity) === capacity).length
          }))

          // Set recent and featured cars
          this.recentCars = this.cars.slice(0, 3)
          this.featuredCar = this.cars[0]

          // Debugging: Check if featuredCar is set correctly
          // eslint-disable-next-line no-console
          console.log('Featured Car:', this.featuredCar)
        }
      } catch (error) {
        // eslint-disable-next-line no-console
        console.error('Error fetching cars:', error)
      }
    },
    async fetchReviews () {
      const response = await this.$axios.get('/reviews/all')
      this.reviews = response.data.reviews
      // eslint-disable-next-line no-console
      console.log('Reviews fetched:', response)
    },
    async fetchClients () {
      const response = await this.$axios.get('/client/all')
      this.clients = response.data.clients
    },
    filterCars () {
      // eslint-disable-next-line no-console
      console.log('Filtering cars...')
      // eslint-disable-next-line no-console
      console.log('Selected types:', this.types.filter(type => type.selected))
      // eslint-disable-next-line no-console
      console.log('Selected capacities:', this.capacities.filter(capacity => capacity.selected))
      // eslint-disable-next-line no-console
      console.log('Price limit:', this.price)

      this.filteredCars = this.cars.filter((car) => {
        // Normaliza las cadenas para comparación uniforme
        const carType = this.normalizeString(car.type)
        const carCapacity = this.normalizeString(car.capacity)

        // Verificar filtros seleccionados
        const typeSelected = this.types.some(type => type.selected)
        const capacitySelected = this.capacities.some(capacity => capacity.selected)

        // Coincidencias con normalización
        const typeMatch = typeSelected
          ? this.types.some(type => type.selected && carType === this.normalizeString(type.name))
          : true
        const capacityMatch = capacitySelected
          ? this.capacities.some(capacity => capacity.selected && carCapacity === this.normalizeString(capacity.name))
          : true
        const priceMatch = car.rentPrice <= this.price

        // Incluir carros si coinciden con las condiciones de los filtros
        return typeMatch && capacityMatch && priceMatch
      })

      // eslint-disable-next-line no-console
      console.log('Filtered cars:', this.filteredCars)
    },
    normalizeString (str) {
      if (typeof str !== 'string') { return str } // Si no es una cadena, devolver sin cambios
      return str.toLowerCase().trim() // Convierte a minúsculas y elimina espacios extras
    },
    navigateToRentForm (carId) {
      // Redirect to the rentForm page with the selected car's ID
      this.$router.push({ name: 'rentForm', query: { carId } })
    }
  }
}
</script>

<style scoped>
.border {
  border: 1px solid #ccc;
}
</style>
