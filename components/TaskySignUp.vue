<template>
  <div class="tasky-sign-in-form mt-4">
    <v-row>
      <v-col class="cols=3"></v-col>
      <v-col class="cols-3 pr-0">
        <v-card class="tasky-sign-in-form-card-1">
          <p class="tasky-sign-in-form-card-1-text">¡Bienvenido a TASKY!</p>
        </v-card>
      </v-col>
      <v-col class="cols-3 pl-0">
        <v-card class="tasky-sign-in-form-card-2">
          <v-card-title
            class="tasky-sign-in-form-card-title d-flex justify-center"
            >Registrate</v-card-title
          >
          <v-card-text class="tasky-sign-in-form-card-text">
            <v-text-field
              v-model="email"
              color="#E0F2F1"
              background-color="#00695C"
              class="tasky-sign-in-form-card-text-field1"
              label="Email"
              required
              outlined
            ></v-text-field>
            <v-text-field
              v-model="password"
              color="#E0F2F1"
              background-color="#00695C"
              class="tasky-sign-in-form-card-text-field2"
              label="Contraseña"
              type="password"
              required
              outlined
            ></v-text-field>
            <v-text-field
              v-model="password2"
              color="#E0F2F1"
              background-color="#00695C"
              class="tasky-sign-in-form-card-text-field2"
              label="Repite Contraseña"
              type="password"
              required
              outlined
            ></v-text-field>
            <div class="tasky-sign-in-buttons d-flex justify-center">
              <v-btn
                type="button"
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
      <v-col class="cols=3">
        <img class="image-main-1" src="/images/taskyimage3.png" alt="" />
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      password2: '',
    }
  },

  methods: {
    async onClick() {
      if (
        !this.email ||
        !this.password ||
        !this.password2 ||
        this.password !== this.password2
      ) {
        alert('Email o password incorrecto')
        return
      }
      if (this.password !== this.password2) {
        alert('Las contraseñas no coinciden')
        return
      }
      try {
        const url = 'http://localhost:4500/api/user/signUp'
        const body = {
          email: this.email,
          password: this.password,
          password2: this.password2,
        }

        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(body),
        })
        
        const data = await res.json()
        
        
        if (data.error) {
          alert(data.error)
        } else {
          this.$router.push(`/verify-code/${data.data._id}`)
        }
      } catch (err) {
        alert(err.message)
      }
    },
  },
}
</script>

<style scoped>
.tasky-sign-in-form-card-1 {
  background-color: #80cbc4;
  margin-top: 65px;
  height: 510px;
}

.tasky-sign-in-form-card-1-text {
  padding-top: 170px;
  font-size: 50px;
  text-align: center;
}

.tasky-sign-in-form-card-2 {
  background-color: #80cbc4;
  margin-top: 65px;
  padding: 20px;
  padding-bottom: 10px;
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

.image-main-1 {
  align-content: center;
  width: 350px;
  height: 500px;
  margin-top: 75px;
  margin-left: 70px;
}
</style>
