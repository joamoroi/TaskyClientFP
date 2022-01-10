<template>
  <div class="tasky-my-area">
    <TaskyAppBar :header="true"></TaskyAppBar>
    <v-container>
      <v-row>
        <v-col cols="3" offset="1">
          <v-dialog transition="dialog-top-transition" width="600"> </v-dialog>
          <v-card class="tasky-container-create-task">
            <TaskyCard title="Crea Nuevo Espacio" icon="mdi-image-filter-none">
              <div class="tasky-my-area-buttons d-flex justify-center">
                <TaskyNewTask
                  title="Nuevo Espacio Personal"
                  name_task="Añade Nuevo Espacio Personal"
                  type="personal"
                ></TaskyNewTask>
              </div>
              <div class="tasky-my-area-buttons d-flex justify-center">
                <TaskyNewTask
                  title="Nuevo Espacio Profesional"
                  color="#1B5E20"
                  name_task="Añade Nuevo Espacio Profesional"
                  type="profesional"
                ></TaskyNewTask>
              </div>
            </TaskyCard>
          </v-card>
          <img class="image-main-1" src="/images/taskyimage5.png" alt="" />
        </v-col>
        <v-col cols="7 ml-0" offset="4">
          <v-card class="tasky-container-myspace">
            <TaskyCard title="Mis Espacios" icon="mdi-google-circles-extended">
              <div class="tasky-myspaces-container">
                <v-row class="pa-2">
                  <v-col
                    cols="6"
                    v-for="task in tasks"
                    :key="task.id"
                    class="tasky-myspaces-container-item mt-n3"
                  >
                    <v-card class="my-spaces-container-card pa-2">
                      <v-img :src="task.image"></v-img>
                      <div class="my-spaces-container-card-title">
                        <v-btn
                          class="
                            my-spaces-container-card-title-button
                            mt-2
                            d-flex
                          "
                          @click="onClick(task._id)"
                          :color="
                            task.type === 'personal' ? '#689F38' : '#1B5E20'
                          "
                          x-large
                          >{{ task.name }}</v-btn
                        >
                      </div>
                      <div
                        class="
                          my-spaces-container-card-button
                          d-flex
                          justify-center
                        "
                      >
                        <v-btn
                          @click="onRemove(task._id)"
                          type="button"
                          class="delete-button ml-1 mt-2"
                          color="#FF5252"
                          small
                          fab
                          >X</v-btn
                        >
                        <TaskyEditTask :title="task.name"></TaskyEditTask>
                      </div>
                    </v-card>
                  </v-col>
                </v-row>
              </div>
            </TaskyCard>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      onFetch: undefined,
    }
  },

  async mounted() {
    await this.loadTasks()
    this.onFetch = setInterval(async () => {
      await this.loadTasks()
    }, 500)
  },

  beforeDestroy() {
    clearInterval(this.onFetch)
  },

  methods: {
    async loadTasks() {
      try {
        const userId = localStorage.getItem('userId')

        const body = {
          userId: userId,
        }

        const res = await fetch('http://localhost:4500/api/task/usertasks', {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(body),
        })

        const data = await res.json()

        if (data.err) {
          console.log(err.message)
          return
        }
        this.tasks = []
        for (const task of data.usertasks) {
          this.tasks.push(task)
        }
        console.log(this.tasks)
      } catch (err) {
        console.log(err.message)
      }
    },
    onClick(taskId) {
      this.$router.push(`/details/${taskId}`)
    },
    async onRemove(taskId) {
      try {
        const body = {
          taskId: taskId,
        }
        await fetch('http://localhost:4500/api/task/remove', {
          method: 'delete',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(body),
        })

        await this.loadTasks()
      } catch (err) {
        console.log(err.message)
      }
    },
  },
}
</script>

<style scoped>
.tasky-container-create-task {
  margin-top: 50px;
}

.tasky-container-myspace {
  margin-top: 50px;
  margin-left: 0px;
}

.image-main-1 {
  align-content: center;
  width: 425px;
  height: 425px;
}

.my-spaces-container-card {
  background-color: #e0f2f1;
}
.my-spaces-container-card-title-button {
  width: 100%;
}

.delete-button {
  font-size: 20px;
}
</style>
