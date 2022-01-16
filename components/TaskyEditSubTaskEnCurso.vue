<template>
  <v-dialog transition="dialog-top-transition" width="600">
    <template v-slot:activator="{ on, attrs }">
      <v-btn
        class="edit-button ml-2 mt-3"
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
            <v-icon color="white" size="25" class="mt-n3">mdi-pencil</v-icon>
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
          <p class="tasky-modal-card-text mt-2">Descripción:</p>
          <v-textarea
            v-model="description"
            placeholder="Descripción"
            outlined
            height="75px"
          ></v-textarea>
          <p class="tasky-modal-card-text mt-2">Fecha vencimiento:</p>
          <TaskyCalendar />
          <TaskyMoveSubTaskEnCurso />
        </v-card-text>
        <div class="d-flex justify-center mt-n3">
          <v-btn
            @click="onUpdateSubtask()"
            class="buttons"
            color="#1DE9B6"
            large
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
    subtaskId: {
      type: String,
    },
  },
  data() {
    return {
      picker: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
        .toISOString()
        .substr(0, 10),
      name: '',
      description: '',
    }
  },
  methods: {
    async onUpdateSubtask() {
      try {
        if (!this.name || !this.description) {
          alert('Campos incompletos')
          return
        }

        const body = JSON.stringify({
          name: this.name,
          description: this.description,
        })

        await fetch(
          `http://localhost:4500/api/subtask/update/${this.subtaskId}`,
          {
            method: 'put',
            headers: {
              'Content-Type': 'application/json',
            },
            body,
          }
        )
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
