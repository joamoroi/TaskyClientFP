<template>
  <div class="tasky-id" v-if="token">
    <TaskyAppBar :remove="true" :header="true"></TaskyAppBar>
    <v-container>
      <v-row>
        <v-col cols="3 offset-2">
          <v-card class="tasky-id-form-card">
            <v-card-title
              class="d-flex justify-center"
              style="font-size: 25px"
              >{{ taskName }}</v-card-title
            >
          </v-card>
        </v-col>

        <img class="image-main-1" src="/images/taskyimage7.png" alt="" />
      </v-row>

      <v-row class="container-tasks">
        <v-col cols="4">
          <v-card class="tasky-id-form-card1">
            <v-card-title>
              <div class="d-flex justify-space-between" style="width: 100%">
                <p class="headline mb-1">TAREAS</p>
                <TaskyNewSubTask type="Tareas"></TaskyNewSubTask>
              </div>
            </v-card-title>
          </v-card>
          <TaskyCard2>
            <div
              v-for="subtask in subtasks"
              :key="subtask.id"
              class="my-tasks-container-card-title d-flex mb-2"
            >
              <TaskyShowSubtasksDetails
                :name_subtask="subtask.name"
                :description="subtask.description"
              ></TaskyShowSubtasksDetails>
              <v-btn
                @click="onRemove(subtask._id)"
                class="delete-button ml-2 mt-3"
                color="#FF5252"
                small
                fab
                style="font-size: 20px"
                >X</v-btn
              >
              <TaskyEditSubTask
                :title="subtask.name"
                :subtaskId="subtask._id"
                :type="subtask.type"
              ></TaskyEditSubTask>
            </div>
          </TaskyCard2>
        </v-col>

        <v-col cols="4">
          <v-card class="tasky-id-form-card2">
            <v-card-title>
              <div class="d-flex justify-space-between" style="width: 100%">
                <p class="headline mb-1">EN CURSO</p>
                <TaskyNewSubTask type="En curso"></TaskyNewSubTask>
              </div>
            </v-card-title>
          </v-card>
          <TaskyCard2>
            <div
              v-for="subtasks_doing in subtasks_doing"
              :key="subtasks_doing.id"
              class="my-tasks-container-card-title d-flex mb-2"
            >
              <TaskyShowSubtasksDetails
                :name_subtask="subtasks_doing.name"
                :description="subtasks_doing.description"
              ></TaskyShowSubtasksDetails>
              <v-btn
                @click="onRemove(subtasks_doing._id)"
                class="delete-button ml-2 mt-3"
                color="#FF5252"
                small
                fab
                style="font-size: 20px"
                >X</v-btn
              >
              <TaskyEditSubTaskEnCurso
                :title="subtasks_doing.name"
                :subtaskId="subtasks_doing._id"
                :type="subtasks_doing.type"
              ></TaskyEditSubTaskEnCurso>
            </div>
          </TaskyCard2>
        </v-col>

        <v-col cols="4">
          <v-card class="tasky-id-form-card3">
            <v-card-title>
              <div class="d-flex justify-space-between" style="width: 100%">
                <p class="headline mb-1">REALIZADAS</p>
                <TaskyNewSubTask type="Realizadas"></TaskyNewSubTask>
              </div>
            </v-card-title>
          </v-card>
          <TaskyCard2>
            <div
              v-for="subtasks_done in subtasks_done"
              :key="subtasks_done.id"
              class="my-tasks-container-card-title d-flex mb-2"
            >
              <TaskyShowSubtasksDetails
                :name_subtask="subtasks_done.name"
                :description="subtasks_done.description"
              ></TaskyShowSubtasksDetails>
              <v-btn
                @click="onRemove(subtasks_done._id)"
                class="delete-button ml-2 mt-3"
                color="#FF5252"
                small
                fab
                style="font-size: 20px"
                >X</v-btn
              >
              <TaskyEditSubTaskRealizadas
                :title="subtasks_done.name"
                :subtaskId="subtasks_done._id"
                :type="subtasks_done.type"
              ></TaskyEditSubTaskRealizadas>
            </div>
          </TaskyCard2>
        </v-col>
      </v-row>
    </v-container>
  </div>
  <div v-else>
    <div class="loader text-center">
      <v-progress-circular
        :size="70"
        :width="7"
        color="green"
        indeterminate
      ></v-progress-circular>
    </div>
  </div>
</template>

<script>
export default {
  props: {},
  data() {
    return {
      subtasks: [],
      onFetch: undefined,

      subtasks_doing: [],

      subtasks_done: [],

      taskName: '',
      token: undefined,
    }
  },

  beforeMount() {
    this.token = localStorage.getItem('token')

    if (!this.token) {
      this.$router.push('/home')
      return
    }
  },

  async mounted() {
    await this.getTask()
    // otra forma de pintar el nombre de la tarea:
    // this.$route.query.taskName
    // console.log({taskName: this.$route.query.taskName})
    await this.loadsubTasks()
    this.onFetch = setInterval(async () => {
      await this.loadsubTasks()
    }, 500)
  },

  beforeDestroy() {
    clearInterval(this.onFetch)
  },

  methods: {
    async loadsubTasks() {
      try {
        const taskId = this.$route.params.id
        const userId = localStorage.getItem('userId')
        const body = {
          taskId: taskId,
          userId: userId,
        }

        const res = await fetch(
          'http://localhost:4500/api/subtask/usersubtasks',
          {
            method: 'post',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify(body),
          }
        )

        const data = await res.json()

        if (data.err) {
          console.log(err.message)
          return
        }
        this.subtasks = []
        this.subtasks_doing = []
        this.subtasks_done = []

        for (const subtask of data.usersubtasks) {
          if (subtask.type === 'Tareas') {
            this.subtasks.push(subtask)
          } else if (subtask.type === 'En curso') {
            this.subtasks_doing.push(subtask)
          } else {
            this.subtasks_done.push(subtask)
          }
        }
        // console.log({ subtask: this.subtasks })
        // console.log({ subtasks_doing: this.subtasks_doing })
        // console.log({ subtasks_done: this.subtasks_done })
      } catch (err) {
        console.log(err.message)
      }
    },
    async onRemove(subtaskId) {
      try {
        const body = {
          subtaskId: subtaskId,
        }
        await fetch('http://localhost:4500/api/subtask/remove', {
          method: 'delete',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(body),
        })

        await this.loadsubTasks()
      } catch (err) {
        console.log(err.message)
      }
    },
    async getTask() {
      try {
        const taskId = this.$route.params.id

        const res = await fetch(
          `http://localhost:4500/api/task/onetask/${taskId}`,
          {
            method: 'GET',
            headers: {
              'Content-Type': 'application/json',
            },
          }
        )

        const data = await res.json()
        // console.log({ data })
        if (data.err) {
          alert(data.err)
        }
        this.taskName = data.onetask.name
      } catch (err) {
        alert(err.message)
      }
    },
  },
}
</script>

<style scoped>
.tasky-id-form-card {
  background-color: #004d40;
  margin-top: 120px;
}

.tasky-id-form-card1 {
  background-color: #1de9b6;
  /* margin-top: 50px; */
}

.tasky-id-form-card2 {
  background-color: #1de9b6;
  /* margin-top: -360px;
    left: 80px;  */
}

.tasky-id-form-card3 {
  background-color: #1de9b6;
  /* margin-top: -385px; */
  /* display: flex;
    justify-content: space-around; */
}

.tasky-id-form-card-text {
  text-align: center;
  font-size: 20px;
}

.add-button {
  text-align: end;
}

.image-main-1 {
  margin-top: 40px;
  margin-bottom: -50px;
  width: 607px;
  height: 271px;
  margin-left: 125px;
}

.container-tasks {
  margin-top: 75px;
}

.loader {
  margin-top: 325px;
}
</style>
