<template>
  <v-app>
      <v-navigation-drawer
        expand-on-hover
        permanent
        rail
      >
        <v-list>
          <v-list-item
            prepend-avatar="https://randomuser.me/api/portraits/men/85.jpg"
            subtitle="anthongarong9@gmail.com"
            title="Anthon Louies D. Garong"
          ></v-list-item>
        </v-list>

        <v-divider></v-divider>

        <v-list density="compact" nav>
          <v-list-item prepend-icon="mdi-view-dashboard" title="Dashboard" to="/"></v-list-item>
          <v-list-item prepend-icon="mdi-qrcode-scan" title="QR Scanner" to="/qr-scanner"></v-list-item>
          <v-list-item prepend-icon="mdi-weather-night" title="Weather" to="/weather"></v-list-item>
        </v-list>
      </v-navigation-drawer>

      <v-app-bar color=black>
    <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
    <v-app-bar-title>System Integration</v-app-bar-title>
  </v-app-bar>


      <v-main> 
        <v-container fluid>
          <slot />
        </v-container>
      </v-main>
    
    </v-app>
</template>

<script lang="ts" setup>

const drawer = ref(true)

</script>

<style>

</style>