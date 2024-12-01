<template>
  <div class="pa-3 ma-3">
    <v-row align="center" justify="center">
      <h1>Administracion de Clientes</h1>
    </v-row>
    <v-row align="center" justify="end">
      <v-btn color="#3282B8" @click="dialogAdd = true">
        <v-icon color="white">
          mdi-plus
        </v-icon>
        <span style="text-transform: none;" class="add-btn-text">
          Agregar Nuevo Contacto
        </span>
      </v-btn>
    </v-row>
    <v-row align="center" justify="center">
      <v-data-table
        :headers="headers"
        :items="clients_data"
        item-key="name"
        class="styled-table rounded-table"
        dense
        hide-default-footer
      >
        <template #[`item.actions`]="{ item }">
          <v-tooltip bottom color="orange">
            <template #activator="{ on, attrs}">
              <v-btn
                icon
                color="warning"
                v-bind="attrs"
                @click="updateClientMid(item)"
                v-on="on"
              >
                <v-icon>
                  mdi-pencil-box-outline
                </v-icon>
              </v-btn>
            </template>
            <span>Actualizar Cliente: {{ item.name }}</span>
          </v-tooltip>
          |
          <v-tooltip bottom color="red">
            <template #activator="{ on, attrs}">
              <v-btn
                icon
                color="red"
                v-bind="attrs"
                @click="deleteClientMid(item.id)"
                v-on="on"
              >
                <v-icon>
                  mdi-trash-can
                </v-icon>
              </v-btn>
            </template>
            <span>Eliminar Cliente: {{ item.name }}</span>
          </v-tooltip>
        </template>
      </v-data-table>
    </v-row>
    <v-dialog v-model="dialogAdd" width="600">
      <v-card class="pa-5 rounded-card" width="600" color="white">
        <v-card-title class="text-h5 text-center mb-4" style="color: #1B262C;">
          Agregar Nuevo Contacto
        </v-card-title>

        <v-card-text>
          <v-form ref="form">
            <v-row dense>
              <!-- Nombre Completo -->
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newClient.fullname"
                  label="Nombre Completo"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>

              <!-- Usuario -->
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newClient.username"
                  label="Usuario"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>

              <!-- Correo Electrónico -->
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newClient.email"
                  label="Correo Electrónico"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>

              <!-- Teléfono -->
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newClient.phone"
                  label="Teléfono"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>

              <!-- Dirección -->
              <v-col cols="12">
                <v-text-field
                  v-model="newClient.address"
                  label="Dirección"
                  outlined
                  dense
                  class="custom-input"
                />
              </v-col>

              <!-- Contraseña -->
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newClient.password"
                  label="Contraseña"
                  type="password"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>

              <!-- Método de Pago -->
              <v-col cols="12" sm="6">
                <v-select
                  v-model="newClient.paymentMethod"
                  :items="metodos"
                  label="Método de Pago"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>

              <!-- Ciudad -->
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newClient.city"
                  label="Ciudad"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>

              <!-- Película Favorita de Cars -->
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newClient.favoriteCars"
                  label="Película Favorita de Cars"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>

              <!-- Foto de Perfil -->
              <v-col cols="12">
                <v-file-input
                  v-model="newClient.image"
                  label="Foto de Perfil"
                  outlined
                  dense
                  prepend-icon="mdi-camera"
                  class="custom-input"
                />
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>

        <v-card-actions class="justify-center mt-4">
          <v-btn color="red darken-1" class="custom-btn mx-2" @click="dialogAdd = false">
            Cancelar
          </v-btn>
          <v-btn color="blue darken-1" class="custom-btn mx-2" @click="addNewClient">
            Agregar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialogUpdate" width="600">
      <v-card class="pa-6 rounded-update-card" width="650" color="grey lighten-4">
        <v-card-title class="text-h5 text-center mb-5" style="color: #34495E;">
          Actualizar Contacto
        </v-card-title>

        <v-card-text>
          <v-form ref="form">
            <v-row dense>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="selectedClient.fullname"
                  label="Nombre Completo"
                  outlined
                  dense
                  class="update-input"
                  required
                />
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="selectedClient.username"
                  label="Nombre de Usuario"
                  outlined
                  dense
                  class="update-input"
                  required
                />
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="selectedClient.email"
                  label="Correo Electrónico"
                  outlined
                  dense
                  class="update-input"
                  required
                />
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="selectedClient.phone"
                  label="Número de Teléfono"
                  outlined
                  dense
                  class="update-input"
                  required
                />
                <v-col cols="12" md="6">
                  <v-text-field
                    v-model="selectedClient.password"
                    label="Número de Teléfono"
                    outlined
                    dense
                    class="update-input"
                    required
                  />
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="selectedClient.address"
                    label="Direccion"
                    outlined
                    dense
                    class="update-input"
                  />
                </v-col>
                <v-col cols="12" sm="6">
                  <v-text-field
                    v-model="newClient.city"
                    label="Ciudad"
                    outlined
                    dense
                    class="custom-input"
                    required
                  />
                </v-col>
                <v-col cols="12" md="6">
                  <v-select
                    v-model="selectedClient.payment_method"
                    :items="metodos"
                    label="Metodo de Pago"
                    outlined
                    dense
                    class="update-input"
                    required
                  />
                </v-col>
                <v-col cols="12" md="6">
                  <v-file-input
                    v-model="selectedClient.image"
                    label="Foto de Perfil"
                    outlined
                    dense
                    prepend-icon="mdi-camera"
                    class="update-input"
                  />
                </v-col>
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>

        <v-card-actions class="d-flex justify-center mt-4">
          <v-btn color="grey darken-1" class="cancel-btn mx-2" @click="dialogUpdate = false">
            Cancelar
          </v-btn>
          <v-btn color="teal darken-1" class="update-btn mx-2" @click="updateClient">
            Actualizar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog
      v-model="dialogDelete"
      width="400"
      persistent
    >
      <v-card class="rounded-lg elevation-3">
        <!-- Título del diálogo -->
        <v-card-title class="text-h6 font-weight-bold text-center text-primary">
          <v-icon color="red" class="mr-2">
            mdi-car
          </v-icon>
          Eliminar Cliente
        </v-card-title>

        <!-- Mensaje del diálogo -->
        <v-card-text class="text-center text-body-1 text-gray-600">
          <v-icon color="blue" class="mb-2" size="36">
            mdi-account-remove
          </v-icon>
          <p>¿Está seguro de que desea eliminar este Cliente? Esta acción no se puede deshacer.</p>
        </v-card-text>

        <!-- Botones de acción -->
        <v-card-actions class="justify-center">
          <v-btn
            color="grey lighten-1"
            class="text-none rounded-pill"
            @click="dialogDelete = false"
          >
            Cancelar
          </v-btn>
          <v-btn
            color="red"
            dark
            class="text-none rounded-pill"
            @click="deleteClient"
          >
            Eliminar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data () {
    return {
      clients_data: [],
      headers: [
        {
          text: 'Nombre Completo',
          align: 'center',
          sortable: true,
          value: 'fullname'
        },
        {
          text: 'Username',
          align: 'center',
          sortable: true,
          value: 'username'
        },
        {
          text: 'ID',
          align: 'center',
          sortable: true,
          value: 'id'
        },
        {
          text: 'Telefono',
          align: 'center',
          sortable: true,
          value: 'phone'
        },
        {
          text: 'Email',
          align: 'center',
          sortable: true,
          value: 'email'
        },
        {
          text: 'Address',
          align: 'center',
          sortable: true,
          value: 'address'
        },
        {
          text: 'City',
          align: 'center',
          sortable: true,
          value: 'city'
        },
        {
          text: 'Acciones',
          align: 'center',
          sortable: true,
          value: 'actions'
        }
      ],
      id: null,
      dialogDelete: false,
      dialogAdd: false,
      dialogUpdate: false,
      newClient: {
        fullname: '',
        username: '',
        email: '',
        phone: '',
        address: '',
        password: '',
        image: null
      },
      selectedClient: {
        fullname: '',
        username: '',
        email: '',
        phone: '',
        address: '',
        password: '',
        image: null
      },
      metodos: [
        'Efectivo',
        'Tarjeta de Credito',
        'Tarjeta de Debito'
      ]
    }
  },
  mounted () {
    this.getEmployees()
  },
  methods: {
    async getEmployees () {
      const res = await this.$axios.get('/client/all')
      if (res && res.data && res.data.success) {
        this.clients_data = res.data.clients
      }
      // eslint-disable-next-line no-console
      console.log('Clientes => ', res)
    },
    deleteClientMid (idDel) {
      this.id = idDel
      this.dialogDelete = true
    },
    async deleteClient () {
      const res = await this.$axios.delete(`/client/delete/${this.id}`)
      // eslint-disable-next-line no-console
      console.log('@Nint res => ', res)
      if (res && res.data && res.data.success) {
        await this.getEmployees()
      }
      this.dialogDelete = false
    },
    async addNewClient () {
      const formData = new FormData()
      for (const key in this.newClient) {
        formData.append(key, this.newClient[key])
      }
      const response = await this.$axios.post('/client/add', formData)
      if (response.data.success) {
        this.newClient = {
          fullname: '',
          username: '',
          email: '',
          phone: '',
          address: '',
          password: '',
          image: null
        }
        this.dialogAdd = false
        await this.getEmployees()
      }
    },
    updateClientMid (client) {
      this.selectedClient = { ...client }
      this.dialogUpdate = true
    },
    async updateClient () {
      const formData = new FormData()
      for (const key in this.selectedClient) {
        formData.append(key, this.selectedClient[key])
      }
      const response = await this.$axios.put(`/client/update/${this.selectedClient.id}`, formData)
      if (response.data.success) {
        this.dialogUpdate = false
        await this.getEmployees()
      }
    }
  }
}
</script>

<style scoped>
.add-btn-text {
  font-size: 16px;
  color: white;
}
.styled-table {
  border-radius: 12px; /* Bordes redondeados */
  border: 1px solid #E0E0E0;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1); /* Elevación */
}
.styled-table .v-data-table-header {
  background-color: #3282B8; /* Fondo azul para los encabezados */
  color: white; /* Color de texto blanco */
}
.styled-table th {
  font-weight: bold;
}
.styled-table .v-data-table__cell {
  border-bottom: 1px solid #ddd; /* Líneas finas entre filas */
}
.styled-table .v-data-table__row:hover {
  background-color: #f5f5f5; /* Color de fondo al pasar el ratón */
}

.rounded-card {
  border-radius: 16px;
}

.custom-input .v-input__control {
  background-color: #F9FAFB !important;
  border-radius: 8px !important;
}

.custom-btn {
  color: white !important;
  border-radius: 24px !important;
  padding: 10px 20px;
}

.v-card-title {
  font-weight: bold;
}
</style>
