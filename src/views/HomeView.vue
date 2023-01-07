<template>
  <div class="section">
    <div class="container" style="max-width: 600px">
      <div class="add-item-form">
        <input
          type="text"
          v-model="task"
          placeholder="Enter task"
          class="w-100 form-control"
        />
        <button class="btn btn-primary" @click="submitTask">
          SUBMIT
        </button>
      </div>

      <!-- Task table -->
      <table class="table table-bordered mt-5">
        <thead>
          <tr>
            <th scope="col">Task</th>
            <th scope="col" style="width: 120px">Status</th>
            <th scope="col" class="text-center">Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <td>
              <span :class="{ 'line-through': task.status === 'finished' }">
                {{ task.name }}
              </span>
            </td>
            <td>
              <span
                class="pointer noselect"
                @click="changeStatus(index)"
                :class="{
                  'text-danger': task.status === 'to-do',
                  'text-success': task.status === 'finished',
                  'text-warning': task.status === 'in-progress',
                }"
              >
                {{ capitalizeFirstChar(task.status) }}
              </span>
            </td>
            <td class="text-center">
              <div @click="deleteTask(index)">
                <span class="fa fa-trash pointer"></span>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'HomeView',
  components: {},
  data () {
    return {
      task: '',
      editedTask: null,
      statuses: ['to-do', 'in-progress', 'finished'],
      tasks: [
        {
          name: 'task1',
          status: 'to-do'
        },
        {
          name: 'task2',
          status: 'in-progress'
        },
        {
          name: 'task3',
          status: 'finished'
        }
      ]
    }
  },
  methods: {
    capitalizeFirstChar (str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    },
    changeStatus (index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status)
      if (++newIndex > 2) newIndex = 0
      this.tasks[index].status = this.statuses[newIndex]
    },
    deleteTask (index) {
      this.tasks.splice(index, 1)
    },
    submitTask () {
      if (this.task.length === 0) return
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null
      } else {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        })
      }
      this.task = ''
    }
  }
}
</script>
<style scoped>
.section {
  background: blue;
  height: 90vh;
  width: 100vw;
  font-family: system-ui, BlinkMacSystemFont, -apple-system, Segoe UI, Roboto, Oxygen, Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container {
    background: #FFF;
    border-radius: 3px;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.12), 0 2px 4px 0 rgba(0,0,0,0.08);
}

.add-item-form, .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.add-item-form input {
    width: 70%;
    border-radius: 3px;
    box-shadow: 0 2px 4px 0 rgba(0,0,0,0.10);
    border: 1px solid #F1F5F8;
    color: #606F7B;
    padding: .5rem .75rem;
    box-sizing: border-box;
    font-size: 1rem;
    letter-spacing: .5px;
    margin: .5rem 0;
}

.btn {
    border: none;
    border-radius: 3px;
    margin: auto 0;
    padding: .5rem .75rem;
    flex-shrink: 0;
    cursor: pointer;
    font-size: .9rem;
    letter-spacing: .5px;
    transition: all .1s ease-in;
}

.btn[disabled] {
    background: #8795A1;
}

.btn[disabled]:hover {
    background: #606F7B;
}

.btn-primary {
    background: #6CB2EB;
    color: #fff;
}

.btn-primary:hover {
    background: #3490DC;
}

.priority {
    color: #DE751F;
}
</style>
