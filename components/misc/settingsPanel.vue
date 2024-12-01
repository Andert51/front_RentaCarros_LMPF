<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-card-title>
            Settings
          </v-card-title>
          <v-card-text>
            <v-list dense>
              <!-- Dark Mode Toggle -->
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Dark Mode</v-list-item-title>
                </v-list-item-content>
                <v-list-item-action>
                  <v-switch v-model="isDarkMode" @change="toggleDarkMode" />
                </v-list-item-action>
              </v-list-item>

              <!-- Sign Out -->
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Sign Out</v-list-item-title>
                </v-list-item-content>
                <v-list-item-action>
                  <v-btn color="red" @click="signOut">
                    Sign Out
                  </v-btn>
                </v-list-item-action>
              </v-list-item>

              <!-- Other Settings -->
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Notification Settings</v-list-item-title>
                </v-list-item-content>
                <v-list-item-action>
                  <v-btn color="primary">
                    Manage
                  </v-btn>
                </v-list-item-action>
              </v-list-item>

              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Privacy Settings</v-list-item-title>
                </v-list-item-content>
                <v-list-item-action>
                  <v-btn color="primary">
                    Manage
                  </v-btn>
                </v-list-item-action>
              </v-list-item>

              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Account Settings</v-list-item-title>
                </v-list-item-content>
                <v-list-item-action>
                  <v-btn color="primary">
                    Manage
                  </v-btn>
                </v-list-item-action>
              </v-list-item>
            </v-list>
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
      isDarkMode: false
    }
  },
  methods: {
    toggleDarkMode () {
      this.isDarkMode = !this.isDarkMode
      // Add logic to toggle dark mode
    },
    signOut () {
      // Add logic to sign out
    }
  }
}
</script>

<style scoped>
</style>
