<template>
  <v-dialog transition="dialog-top-transition" width="600">
    <template v-slot:activator="{ on, attrs }">
      <v-btn class="mb-3 mt-3" large :color="color" v-bind="attrs" v-on="on">{{
        name_task
      }}</v-btn>
    </template>
    <template v-slot:default="dialog">
      <v-card class="tasky-modal-card">
        <v-card-title style="background-color: #00897b">
          <div class="tasky-modal-card-title d-flex">
            <v-icon color="white" size="35" class="mt-n3"
              >mdi-pencil-box-outline</v-icon
            >
            <p class="headline mx-2 white--text">{{ title }}</p>
          </div>
        </v-card-title>
        <v-card-text>
          <p class="tasky-modal-card-text mt-5">Nombre:</p>
          <v-text-field
            v-model="name"
            name="name"
            placeholder="Nuevo Espacio"
            outlined
          ></v-text-field>
          <p class="tasky-modal-card-text">Imagen:</p>
          <v-file-input
            v-model="image"
            name="image"
            outlined
            placeholder="File"
            prepend-icon=""
          ></v-file-input>
        </v-card-text>
        <div class="d-flex justify-center mt-n3">
          <v-btn class="buttons" color="#1DE9B6" large @click="onSubmit"
            >Crear</v-btn
          >
        </div>
        <v-card-actions class="justify-end mt-5">
          <v-btn text @click="dialog.value = false">Close</v-btn>
        </v-card-actions>
      </v-card>
    </template>
  </v-dialog>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
    },
    name_task: {
      type: String,
    },
    color: { type: String, default: '#689F38' },
    type: {
      type: String,
    },
  },

  data() {
    return {
      image: undefined,
      name: '',
      // type: 'personal',
    }
  },

  methods: {
    async onSubmit() {
      if (!this.image || !this.name) {
        alert('Campos incompletos')
        return
      }

      const userId = localStorage.getItem('userId')

      const formData = new FormData()
      formData.enctype = 'multipart/form-data'
      formData.append('image', this.image)
      formData.append('name', this.name)
      formData.append('type', this.type)
      formData.append('userId', userId)

      const token = localStorage.getItem('token')

      try {
        const res = await fetch('http://localhost:4500/api/task/create', {
          method: 'POST',
          headers: {
            token: token,
          },
          body: formData,
        })

        const data = await res.json()
        console.log({ data })
        if (data.err) {
          alert(data.err)
        }
        // window.localStorage.setItem('taskName', data.task.name)
        this.$router.push(`/my-area/${data.task.userId}`)
      } catch (err) {
        alert(err.message)
      }
    },
  },
}
</script>

<style scoped>
.tasky-modal-card {
  background-color: #80cbc4;
}

.tasky-modal-card-text {
  font-size: 20px;
  margin-bottom: 10px;
}

.buttons {
  width: 150px;
  font-size: 18px;
}
</style>
