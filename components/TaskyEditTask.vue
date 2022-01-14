<template>
  <v-dialog transition="dialog-top-transition" width="600">
    <template v-slot:activator="{ on, attrs }">
      <v-btn
        class="edit-button ml-2 mt-2"
        color="#D4E157"
        small
        fab
        v-bind="attrs"
        v-on="on"
      >
        <v-icon size="20">mdi-pencil</v-icon>
      </v-btn>
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
            placeholder="Nuevo Nombre"
            outlined
          ></v-text-field>
          <p class="tasky-modal-card-text">Imagen:</p>
          <v-file-input
            v-model="image"
            outlined
            placeholder="File"
            prepend-icon=""
          ></v-file-input>
        </v-card-text>
        <div class="d-flex justify-center mt-n3">
          <v-btn @click="onUpdate()" class="buttons" color="#1DE9B6" large
            >Guardar</v-btn
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
    taskId: {
      type: String,
    },
  },

  data() {
    return {
      image: undefined,
      name: '',
    }
  },
  methods: {
    async onUpdate() {
      try {
        if (!this.image || !this.name) {
          alert('Campos incompletos')
          return
        }

        const userId = localStorage.getItem('userId')

        const formData = new FormData()
        formData.enctype = 'multipart/form-data'
        formData.append('image', this.image)
        formData.append('name', this.name)
        formData.append('userId', userId)

        await fetch(`http://localhost:4500/api/task/update/${this.taskId}`, {
          method: 'put',
          body: formData,
        })

      } catch (err) {
        console.log(err.message)
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
