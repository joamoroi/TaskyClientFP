<template>
  <div class="tasky-verify-code-form mt-4">
    <v-row>
      <v-col></v-col>
      <v-col class="cols-3 pr-0">
        <v-card class="tasky-verify-code-form-card-1">
          <p class="tasky-verify-code-form-card-1-text">¡Bienvenido a Tasky!</p>
        </v-card>
      </v-col>
      <v-col class="cols-3 pl-0">
        <v-card class="tasky-verify-code-form-card-2">
          <v-card-title
            class="tasky-verify-code-form-card-title d-flex justify-center"
            >Introduce el Código</v-card-title
          >
          <v-card-text class="tasky-verify-code-form-card-text">
            <v-text-field
              v-model="code"
              color="#E0F2F1"
              background-color="#00695C"
              class="tasky-verify-code-form-card-text-field1"
              label="Código"
              outlined
            ></v-text-field>
            <div class="tasky-verify-code-buttons d-flex justify-center">
              <v-btn
                @click="onClick"
                class="mb-3 mt-2 white--text"
                color="#1DE9B6"
                x-large
                >Submit</v-btn
              >
            </div>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col></v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      code: '',
    }
  },

  methods: {
    async onClick() {
      try {
        const userId = this.$route.params.id
        const code = this.code

        if (!code) {
          alert('Ingrese el código')
          return
        }

        const res = await fetch('http://localhost:4500/api/user/active', {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            code,
            userId,
          }),
        })

        const data = await res.json()
        if (data.error) {
          alert(data.error)
          return
        }

        //token//

        this.$router.push('/sign-in')
      } catch (err) {
        alert(err.message)
      }
    },
  },
}
</script>

<style scoped>
.tasky-verify-code-form-card-1 {
  background-color: #80cbc4;
  margin-top: 100px;
  height: 316px;
}

.tasky-verify-code-form-card-1-text {
  padding-top: 125px;
  font-size: 35px;
  text-align: center;
}

.tasky-verify-code-form-card-2 {
  background-color: #80cbc4;
  margin-top: 100px;
  padding: 20px;
}

.tasky-verify-code-form-card-title {
  font-size: 30px !important;
  padding: 35px;
}

.tasky-verify-code-form-card-text-field1 .v-label {
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
