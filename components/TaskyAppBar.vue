<template>
  <div class="tasky-app-bar">
    <v-app-bar app color="#00897B" height="75px">
      <v-btn icon>
        <v-icon size="38">mdi-newspaper</v-icon>
      </v-btn>
      <div
        v-if="remove"
        class="d-flex justify-space-between"
        style="width: 100%"
      >
        <p class="tasky-app-bar-title my-1 pl-2">TASKY</p>
        <v-spacer></v-spacer>
        <v-btn to="/my-area" color="#1DE9B6" class="my-2 mx-4"
          >Mis Espacios</v-btn
        >
        <v-btn @click="logout" color="#1DE9B6" class="my-2">Cerrar Sesión</v-btn>
      </div>
      <div
        v-else-if="header"
        class="d-flex justify-space-between"
        style="width: 100%"
      >
        <p class="tasky-app-bar-title my-1 pl-2">TASKY</p>
        <v-btn @click="logout" color="#1DE9B6" class="my-2">Cerrar Sesión</v-btn>
      </div>
      <div
        v-else-if="back"
        class="d-flex justify-space-between"
        style="width: 100%"
      >
        <p class="tasky-app-bar-title my-1 pl-2">TASKY</p>
        <v-btn to="/home" color="#1DE9B6" class="my-2">Home</v-btn>
      </div>
      <div v-else class="d-flex justify-space-between" style="width: 100%">
        <p class="tasky-app-bar-title my-1 pl-2">TASKY</p>
      </div>
    </v-app-bar>
  </div>
</template>

<script>
export default {
  props: {
    remove: {
      type: Boolean,
      default: false,
    },
    header: {
      type: Boolean,
      default: false,
    },
    back: {
      type: Boolean,
      default: true,
    },
  },
  methods: {
    logout() {
      this.$store.dispatch('user/removeToken')
      this.$router.push('/home')
    },
  },
}
</script>

<style scoped>
.tasky-app-bar-title {
  font-size: 30px !important;
}
</style>
