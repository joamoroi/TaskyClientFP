<template>
  <v-dialog transition="dialog-top-transition" width="600">
    <template v-slot:activator="{ on, attrs }">
      <v-btn class="add-button" color="#00897B" v-bind="attrs" v-on="on">
        <v-icon size="30">mdi-plus</v-icon>
      </v-btn>
    </template>
    <template v-slot:default="dialog">
      <v-card class="tasky-modal-card">
        <v-card-title style="background-color: #00897b">
          <div class="tasky-modal-card-title d-flex">
            <v-icon color="white" size="35" class="mt-n3"
              >mdi-plus-circle-outline</v-icon
            >
            <p class="headline mx-2 white--text">AÑADIR TAREA</p>
          </div>
        </v-card-title>
        <v-card-text>
          <p class="tasky-modal-card-text mt-5">Nombre:</p>
          <v-text-field
            v-model="name"
            name="name"
            placeholder="Nuevo Nombre"
            outlined
          ></v-text-field>
          <p class="tasky-modal-card-text mt-2">Descripción:</p>
          <v-textarea
            v-model="description"
            name="description"
            placeholder="Descripción"
            outlined
            height="75px"
          ></v-textarea>
          <p class="tasky-modal-card-text mt-2">Fecha vencimiento:</p>
          <TaskyCalendar :model.sync="date"></TaskyCalendar>
        </v-card-text>
        <div class="d-flex justify-center mt-n3">
          <v-btn class="buttons" color="#1DE9B6" large @click="onSubmit(dialog)"
            >Añadir</v-btn
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
    type: {
      type: String,
    },
    task_name: {
      type: String,
    },
  },
  data() {
    return {
      date: '',
      name: '',
      description: '',
    }
  },

  methods: {
    async onSubmit(dialog) {
      try {
        if (!this.name || !this.description) {
          alert('Campos incompletos')
          return
        }
        const taskId = this.$route.params.id
        
        const body = JSON.stringify({
          name: this.name,
          description: this.description,
          taskId: taskId,
          type: this.type,
          date: this.date,
        })

        const token = localStorage.getItem('token')

        const res = await fetch('http://localhost:4500/api/subtask/create', {
          method: 'POST',
          headers: {
            token: token,
            'Content-Type': 'application/json',
          },
          body,
        })

        const data = await res.json()
        console.log(data)

        if (data.err) {
          alert(data.err)
        }
        //Limpiar modal
        this.name = ''
        this.description = ''

        //cerrar modal al darle añadir pasandole dialog a la funcion
        dialog.value = false
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
