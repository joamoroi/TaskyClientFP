<template>
  <div class="tasky-sign-in-form mt-4">
    <v-row>
      <v-col col-lg-4->
        <img class="image-main-1 mx-6" src="/images/taskyimage4.png" alt="" />
      </v-col>
      <v-col class="cols-3 pr-0">
        <v-card class="tasky-sign-in-form-card-1">
          <p class="tasky-sign-in-form-card-1-text">¡Bienvenido de Vuelta!</p>
        </v-card>
      </v-col>
      <v-col class="cols-3 pl-0">
        <v-card class="tasky-sign-in-form-card-2">
          <v-card-title
            class="tasky-sign-in-form-card-title d-flex justify-center"
            >Inicia Sesión</v-card-title
          >
          <v-card-text class="tasky-sign-in-form-card-text">
            <v-text-field
              v-model="email"
              color="#E0F2F1"
              background-color="#00695C"
              class="tasky-sign-in-form-card-text-field1"
              label="Email"
              outlined
              required
            ></v-text-field>
            <v-text-field
              v-model="password"
              color="#E0F2F1"
              background-color="#00695C"
              class="tasky-sign-in-form-card-text-field2"
              label="Contraseña"
              type="password"
              outlined
              required
            ></v-text-field>
            <div class="tasky-sign-in-buttons d-flex justify-center">
              <v-btn
                @click="onClick"
                class="mb-3 mt-2 white--text"
                color="#1DE9B6"
                x-large
                >Submit</v-btn
              >
            </div>
            <div class="tasky-link-home">
              <nuxt-link to="/home">Volver</nuxt-link>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col class="cols=2"> </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
    }
  },
  methods: {
    async onClick() {
      try {
        if (!this.email || !this.password) {
          alert('Email o password incorrecto')
          return
        }
        const body = JSON.stringify({
          email: this.email,
          password: this.password,
        })
        const res = await fetch('http://localhost:4500/api/user/signIn', {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body,
        })
        const data = await res.json()
        // console.log({data})
        if (data.error) {
          alert(data.error)
          return
        }
        // Almacenar token:
        const payload = {
          token: data.token,
        }
        window.localStorage.setItem('token', payload.token)
        window.localStorage.setItem('userId', data.userId)
        this.$router.push(`/my-area/${data.userId}`)  
      } catch (err) {
        alert(err.message)
      }
    },
  },
}
</script>

<style scoped>
.image-main-1 {
  align-content: center;
  width: 470px;
  height: 460px;
  margin-top: 75px;
}

.tasky-sign-in-form-card-1 {
  background-color: #80cbc4;
  margin-top: 100px;
  height: 434px;
}

.tasky-sign-in-form-card-1-text {
  padding-top: 135px;
  font-size: 50px;
  text-align: center;
}

.tasky-sign-in-form-card-2 {
  background-color: #80cbc4;
  margin-top: 100px;
  padding: 20px;
}

.tasky-sign-in-form-card-title {
  font-size: 35px !important;
  padding: 35px;
}

.tasky-sign-in-form-card-text-field1 .v-label {
  color: #00695c !important;
}

.tasky-sign-in-form-card-text-field2 .v-label {
  color: #00695c !important;
}

.tasky-link-home {
  text-align: center;
  margin-top: 10px;
}

.v-application a {
  color: #00695c;
}
</style>
