<template>
  <v-app :dark="isDarkTheme">
    <v-navigation-drawer
      app
      permanent
      class="white"
      width="290"
    >
      <v-list nav dense>
        <!-- Logo -->
        <v-list-item>
          <v-img src="/logo.jpg" max-height="40" contain class="mx-auto" />
        </v-list-item>

        <h1 class="main-menu-title">
          MAIN MENU
        </h1>

        <v-divider class="my-4" />

        <!-- Menu Items -->
        <v-list-item-group>
          <v-list-item
            v-for="(item, index) in menuItems"
            :key="index"
            class="menu-item"
            link
            :to="item.path"
          >
            <v-list-item-icon>
              <v-icon class="menu-icon">
                {{ item.icon }}
              </v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>

        <v-divider class="my-4" />

        <v-list-item-group>
          <v-list-item
            v-for="(item, index) in extraItems"
            :key="index"
            class="menu-item"
            link
            :to="item.path"
          >
            <v-list-item-icon>
              <v-icon class="menu-icon">
                {{ item.icon }}
              </v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>

        <!-- Preferences Section -->
        <v-divider class="my-4" />
        <v-list-item-group>
          <v-list-item link>
            <v-list-item-icon>
              <v-icon>mdi-help-circle</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>Centro de Ayuda</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item @click="togleTheme">
            <v-list-item-icon>
              <v-icon>mdi-theme-light-dark</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>Modo Obscuro</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item link @click="gotoLogOut">
            <v-list-item-icon>
              <v-icon>mdi-logout</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>LogOut</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app flat :color="isDarkTheme ? 'grey darken-4' : 'white'">
      <!-- Logo -->
      <v-toolbar-title :class="isDarkTheme ? 'text-white' : 'text-primary'">
        MORENT
      </v-toolbar-title>

      <!-- Search Bar -->
      <v-spacer />
      <v-text-field
        rounded
        dense
        hide-details
        placeholder="Search something here"
        prepend-inner-icon="mdi-magnify"
        class="search-bar"
        :class="isDarkTheme ? 'text-white' : ''"
      />

      <!-- Icons -->
      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>mdi-bell</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>mdi-cog</v-icon>
      </v-btn>

      <v-avatar size="36">
        <img src="https://randomuser.me/api/portraits/lego/6.jpg" alt="User Avatar">
      </v-avatar>
    </v-app-bar>

    <!-- Main Content -->
    <v-main>
      <v-container fluid>
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      isDarkTheme: false,
      menuItems: [
        { title: 'Dashboard', icon: 'mdi-view-dashboard', path: '/dashboard' },
        { title: 'Car Rent', icon: 'mdi-car', path: '/dashboard/carRent' },
        { title: 'Insight', icon: 'mdi-chart-bar', path: '/dashboard/insight' },
        { title: 'Clients', icon: 'mdi-cash-refund', path: '/dashboard/clients' },
        { title: 'Cars', icon: 'mdi-car-multiple', path: '/dashboard/cars' },
        { title: 'Inbox', icon: 'mdi-email', path: '/dashboard/inbox' },
        { title: 'Calendar', icon: 'mdi-calendar', path: '/dashboard/calendar' },
        { title: 'Settings', icon: 'mdi-cog', path: '/dashboard/settings' }
      ],
      extraItems: [
        { title: 'Profile', icon: 'mdi-account', path: '/dashboard/profile' },
        { title: 'Notifications', icon: 'mdi-bell', path: '/dashboard/notifications' }
      ]
    }
  },
  methods: {
    async gotoLogOut () {
      await this.$auth.logout()
      this.$router.push('/')
    },
    togleTheme () {
      this.isDarkTheme = !this.isDarkTheme
    }
  }
}
</script>

<style scoped>

.main-menu-title {
  font-family: "Josefin Sans", sans-serif;
  font-optical-sizing: auto;
  font-size: 14px;
  font-weight: 500;
  margin: 16px;
  color: rgb(83, 81, 81);
}

.search-bar {
  max-width: 300px;
  margin-right: 16px;
}

.menu-item {
  font-family: "Josefin Sans", sans-serif;
  font-optical-sizing: auto;
  padding: 5px 11px;
  margin: 8px 16px;
  transition: background-color 0.3s;
}

.menu-item:hover {
  background-color: #3563E9;
}

.menu-item .v-list-item__icon {
  color: purple;
}

.menu-icon {
  color: #3563E9;
  border: #3563E9;
}

.menu-item:active, .menu-item--active {
  background-color: #3563E9;
}

.text-primary {
  color: #1976D2 !important;
}
.text-white {
  color: white !important;
}
.search-bar .v-input__control {
  color: inherit !important;
}

</style>
