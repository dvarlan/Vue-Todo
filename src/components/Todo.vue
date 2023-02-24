<template>
  <div id="appWrapper">
    <h1>Vue Todo List</h1>
    <h3>Completed: {{ completed }}/{{ todos.length }}</h3>
    <input id="inputField" type="text" placeholder="Enter a new task..." v-model="input">
    <button class="btn" @click="addTodo">Submit</button>
    <div id="tableWrapper">
      <table>
          <tr>
            <th>Task</th>
            <th>Status</th>
          </tr>
        <tr v-for="(task, index) in todos" :key="index">
          <th :class="getStatus(index)">{{ task.title }}</th>
          <th>{{ task.status }}</th>
          <th><button class="btn" @click="changeStatus(index)" ref="btn">✓</button></th>
          <th><button class="btn" @click="deleteTask(index)">Delete</button></th>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoComponent',
  beforeMount() {
    // Handle saving & loading of the todos
    window.addEventListener("beforeunload", this.saveList)
    if (window.localStorage.length !== 0 && window.localStorage.getItem("list") !== this.todos) {
      this.todos = JSON.parse(window.localStorage.getItem("list"))
    }
  },
  mounted() {
    // Check if the status is correct & calculate completed tasks
    this.calcCompleted()
    this.checkStatus()
  },
  data() {
    return {
      input: '',
      completed: 0,
      todos: [{
        title: 'Create a new todo',
        status: 'Todo'
      }]
    }
  },
  methods: {
    addTodo() {
      if (this.input === '') {
        console.log("Empty input!")
        alert("The title of a task can't be empty!")
        return
      } else {
        this.todos.push({ title: this.input, status: 'Todo' })
      }
    },
    changeStatus(index) {
      // Change the status, button text
      if (this.todos[index].status === 'Todo') {
        this.todos[index].status = 'Done'
        this.$refs.btn[index].innerText = 'X'
      } else {
        this.todos[index].status = 'Todo'
        this.$refs.btn[index].innerText = '✓'
      }
      this.calcCompleted()
    },
    deleteTask(index) {
      this.todos.splice(index, 1)
      this.calcCompleted()
      this.checkStatus()
    },
    calcCompleted() {
      // Calculates the number of completed tasks
      let count = 0
      for (let i = 0; i < this.todos.length; ++i) {
        if (this.todos[i].status === 'Done') {
          count++
        }
      }
      this.completed = count
    },
    checkStatus() {
      // Check if all buttons have the correct symbol e.g. after a delete operation
      for (let i = 0; i < this.todos.length; ++i) {
        if (this.todos[i].status === 'Done') {
          this.$refs.btn[i].innerText = 'X'
        } else {
          this.$refs.btn[i].innerText = '✓'
        }
      }
    },
    getStatus(index) {
      if (this.todos[index].status === 'Done') {
        return "doneTodo"
      } else {
        return "openTodo"
      }
    },
    saveList() {
      window.localStorage.setItem("list", JSON.stringify(this.todos))
    }
  }
}
</script>

<style scoped>
#appWrapper {
  text-align: center;
  font-size: larger;
}

#tableWrapper {
  margin-top: 22.464px;
}

.doneTodo {
  text-decoration: line-through;
}

.btn {
  font-family: inherit;
  color: inherit;
  font-size: 15px;
  background: none;
  border: 1px solid whitesmoke;
  border-radius: 3px;
  padding: 4px 7px;
  margin-left: 10px;
}

.btn:hover {
  background-color: #323232;
}

#inputField {
  font-family: inherit;
  font-size: 15px;
  color: inherit;
  height: 20px;
  background-color: inherit;
  border: 1px solid whitesmoke;
  border-radius: 3px;
  padding: 4px 7px;
}

#inputField:focus {
  border: 2px solid whitesmoke;
  background-color: #323232;
  outline: none;
}
</style>
