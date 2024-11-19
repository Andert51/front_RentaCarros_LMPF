<template>
  <v-app>
    <v-navigation-drawer
      app
      permanent
      class="white"
      width="260"
    >
      <v-list>
        <!-- Logo -->
        <v-list-item>
          <v-img src="/logo.png" max-height="40" contain class="mx-auto" />
        </v-list-item>

        <v-divider class="my-4" />

        <!-- Menu Items -->
        <v-list-item-group>
          <v-list-item v-for="item in menuItems" :key="item.title" link>
            <v-list-item-icon>
              <v-icon>{{ item.icon }}</v-icon>
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
              <v-icon>mdi-cog</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>Opciones</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item link>
            <v-list-item-icon>
              <v-icon>mdi-help-circle</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>Help & Center</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-theme-light-dark</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>Dark Mode</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>

        <v-divider class="my-4" />
        <v-list-item link>
          <v-list-item-icon>
            <v-icon>mdi-logout</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>Log Out</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app flat color="white">
      <!-- Logo -->
      <v-toolbar-title class="text-primary">
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
      />

      <!-- Icons -->
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
      menuItems: [
        { title: 'Dashboard', icon: 'mdi-view-dashboard' },
        { title: 'Car Rent', icon: 'mdi-car' },
        { title: 'Insight', icon: 'mdi-chart-bar' },
        { title: 'Clients', icon: 'mdi-cash-refund' },
        { title: 'Inbox', icon: 'mdi-email' },
        { title: 'Calendar', icon: 'mdi-calendar' }
      ]
    }
  }
}
</script>

<style scoped>
.search-bar {
  max-width: 300px;
  margin-right: 16px;
}
</style>
