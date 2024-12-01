<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-card-title>
            Add a Review
          </v-card-title>
          <v-card-text>
            <v-form ref="form">
              <v-row>
                <v-col cols="12">
                  <v-select
                    v-model="newReview.clientId"
                    :items="clients"
                    item-text="fullname"
                    item-value="id"
                    label="Select Client"
                    outlined
                    required
                  />
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="newReview.reviewText"
                    label="Comment"
                    outlined
                    required
                  />
                </v-col>
                <v-col cols="12">
                  <v-rating
                    v-model="newReview.rating"
                    label="Stars"
                    required
                  />
                </v-col>
              </v-row>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn color="blue darken-1" text @click="addReview">
              Add Review
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-data-table
          :headers="headers"
          :items="formattedReviews"
          item-key="id"
          class="elevation-1"
        >
          <template #[`item.actions`]="{ item }">
            <v-btn icon @click="deleteReview(item.id)">
              <v-icon color="red">
                mdi-delete
              </v-icon>
            </v-btn>
          </template>
        </v-data-table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      reviews: [],
      clients: [],
      newReview: {
        clientId: null,
        reviewText: '',
        rating: 0,
        createdAt: null
      },
      headers: [
        { text: 'Client', value: 'clientName' },
        { text: 'Comment', value: 'reviewText' },
        { text: 'Stars', value: 'rating' },
        { text: 'Date', value: 'createdAt' },
        { text: 'Actions', value: 'actions', sortable: false }
      ]
    }
  },
  computed: {
    formattedReviews () {
      return this.reviews.map((review) => {
        const client = this.clients.find(client => client.id === review.clientId)
        return {
          ...review,
          clientName: client ? client.fullname : 'Unknown',
          createdAt: new Date(review.createdAt).toLocaleString()
        }
      })
    }
  },
  async mounted () {
    await this.fetchClients()
    await this.fetchReviews()
  },
  methods: {
    async fetchClients () {
      const response = await this.$axios.get('/client/all')
      this.clients = response.data.clients
    },
    async fetchReviews () {
      const response = await this.$axios.get('/reviews/all')
      this.reviews = response.data.reviews
    },
    async addReview () {
      this.newReview.createdAt = new Date().toISOString()
      const response = await this.$axios.post('/reviews/add', this.newReview)
      if (response.data.success) {
        this.newReview.clientId = null
        this.newReview.reviewText = ''
        this.newReview.rating = 0
        this.newReview.createdAt = null
        await this.fetchReviews()
      }
    },
    async deleteReview (reviewId) {
      const response = await this.$axios.delete(`/reviews/delete/${reviewId}`)
      if (response.data.success) {
        await this.fetchReviews()
      }
    }
  }
}
</script>

<style scoped>
</style>
