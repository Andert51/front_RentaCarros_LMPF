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
              <v-checkbox v-model="type.selected" :label="`${type.name} (${type.count})`" />
            </v-list-item>
          </v-list>

          <v-divider class="my-2" />

          <v-card-title class="text-h6 font-weight-bold">
            Capacity
          </v-card-title>
          <v-list dense>
            <v-list-item v-for="capacity in capacities" :key="capacity.name" class="py-1">
              <v-checkbox v-model="capacity.selected" :label="`${capacity.name} (${capacity.count})`" />
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
                    src="/car-featured.png"
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
                  <v-card-title class="text-h5 font-weight-bold mb-3">
                    Nissan GT - R
                  </v-card-title>
                  <v-rating value="4.5" color="amber" dense readonly />
                  <p class="text-body-2">
                    NISMO has become the embodiment of Nissan's outstanding performance.
                  </p>
                  <v-row>
                    <v-col cols="6">
                      <v-chip class="ma-1" outlined small>
                        {{ featuredCar.type }}
                      </v-chip>
                      <v-chip class="ma-1" outlined small>
                        {{ featuredCar.capacity }}
                      </v-chip>
                    </v-col>
                    <v-col cols="6">
                      <p class="text-h6 font-weight-bold">
                        $80.00 / day
                      </p>
                      <p class="text-body-2 text--line-through">
                        $100.00
                      </p>
                    </v-col>
                  </v-row>
                  <v-btn color="primary" block>
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
                <v-list-item v-for="review in reviews" :key="review.name" class="py-1">
                  <v-list-item-avatar>
                    <img :src="review.avatar" alt="avatar">
                  </v-list-item-avatar>
                  <v-list-item-content>
                    <v-list-item-title class="text-h6">
                      {{ review.name }}
                    </v-list-item-title>
                    <v-list-item-subtitle class="text-body-2">
                      {{ review.text }}
                    </v-list-item-subtitle>
                  </v-list-item-content>
                  <v-rating value="review.rating" readonly color="amber" dense />
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
                  v-for="car in recentCars"
                  :key="car.name"
                  cols="4"
                >
                  <v-card>
                    <v-img :src="car.image" aspect-ratio="16/9" />
                    <v-card-title class="text-h6 font-weight-bold">
                      {{ car.name }}
                    </v-card-title>
                    <v-card-subtitle class="text-body-2">
                      {{ car.price }} / day
                    </v-card-subtitle>
                    <v-btn color="primary" block small>
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
  data () {
    return {
      // Filters
      types: [
        { name: 'Sport', count: 10, selected: false },
        { name: 'SUV', count: 12, selected: false },
        { name: 'MPV', count: 16, selected: false }
      ],
      capacities: [
        { name: '2 Person', count: 10, selected: false },
        { name: '4 Person', count: 12, selected: false }
      ],
      price: 100,

      // Featured Car
      featuredCar: {
        name: 'Nissan GT - R',
        type: 'Sport',
        capacity: '2 Person',
        price: '$80.00',
        images: ['/nissangt.jpg']
      },

      // Reviews
      reviews: [
        {
          name: 'Alex Stanton',
          text: 'Great service from the MORENT app.',
          rating: 5,
          avatar: '/user1.jpg'
        }
      ],

      // Recent Cars
      recentCars: [
        { name: 'Koenigsegg', price: '$99.00', image: '/car4.png' }
      ]
    }
  }
}
</script>

<style scoped>
.border {
  border: 1px solid #ccc;
}
</style>
