<template>
  <v-dialog transition="dialog-top-transition" width="600">
    <template v-slot:activator="{ on, attrs }">
      <v-btn class="mt-2" x-large :color="getColor" v-bind="attrs" v-on="on">
        {{ name_subtask }}
      </v-btn>
    </template>
    <template v-slot:default="dialog">
      <v-card class="tasky-modal-card">
        <v-card-title style="background-color: #00897b">
          <div class="tasky-modal-card-title d-flex">
            <v-icon color="white" size="35" class="mt-n3"
              >mdi-file-document-multiple-outline</v-icon
            >
            <p class="headline mx-2 white--text">DETALLES TAREA</p>
          </div>
        </v-card-title>
        <v-card-text>
          <p class="tasky-modal-card-text mt-5">Nombre:</p>
          <v-text-field readonly v-model="name_subtask" name="name" outlined>
            {{ name_subtask }}
          </v-text-field>
          <p class="tasky-modal-card-text mt-2">Descripción:</p>
          <v-textarea
            readonly
            v-model="description"
            name="description"
            outlined
            height="75px"
            >{{ description }}</v-textarea
          >
          <p class="tasky-modal-card-text mt-2">Fecha vencimiento:</p>
          <v-text-field v-if="date_subtask" readonly v-model="date_subtask" name="date" outlined>
            {{ date_subtask }}
          </v-text-field>
          <v-text-field v-else readonly v-model="date_subtask_realizadas" name="date" outlined>
            {{ date_subtask_realizadas }}
          </v-text-field>
        </v-card-text>
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
    description: {
      type: String,
    },
    name_subtask: {
      type: String,
    },
    date_subtask: {
      type: String,
    },
    date_subtask_realizadas: {
      type: String,
    },
  },
  computed: {
    getColor() {
      if (!this.date_subtask) {
        return '#00897B'
      }
      if (this.date_subtask_realizadas) {
        return '#00897B'
      }
      const redColor = '#d63031'
      const yellowColor = '#fdcb6e'
      const dateNow = new Date()
      const dateExpired = new Date(this.date_subtask)
      console.log(dateNow, dateExpired)
      if (dateNow.getTime() > dateExpired.getTime()) {
        return redColor
      }
      dateNow.setDate(dateNow.getDate() + 1)
      if (dateNow.getTime() > dateExpired.getTime()) {
        return yellowColor
      }
      return '#00897B'
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
