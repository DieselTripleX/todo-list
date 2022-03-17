<template>
  <h1>Hello!!! It's my simple todo-list!</h1>
  <div class="new-task_form">
    <input type="text" v-model="newTaskName">
    <button @click="addNewTask">+</button>
  </div>
  <div class="tasks-list">
    <div class="task" v-for="(task, index) in tasks" :key="index">
      <input @click="checkTask(task)" type="checkbox" name="task" :checked="task.checked">
      <label for="task">{{ (index + 1) + '.' + task.taskName }}</label>
      <button @click="removeTask(index)">delete</button>
    </div>
  </div>
  <div class="empty"></div>
</template>

<script>
  export default {
    name: 'TodoList',
    data() {
      return {
        newTaskName: 'What`s the task?',
        tasks: [
          // { taskName: 'Task1', checked: true },
          // { taskName: 'Task2', checked: false }
        ]
      }
    },
    watch: {
      newTaskName() {
        document.title = this.newTaskName // Techically, you are editing folder title)))
      },
      tasks: {
        handler(){
          window.sessionStorage.setItem('todo-list', JSON.stringify(this.tasks))
        },
        deep: true
      }
    },
    methods: {
      addNewTask() {
        this.tasks.push({ taskName: this.newTaskName, checked: false });
      },
      removeTask(taskIndex) {
        this.tasks.splice(taskIndex, 1);
      },
      checkTask(task) {
        task.checked = !task.checked;
      },
      loadTasks() {
        try {
          const loadedList = window.sessionStorage.getItem('todo-list')
          if (loadedList !== null) {
            this.tasks = JSON.parse(loadedList)
          }
        } catch(e) {
          console.log(e)
        }
      }
    },
    mounted() {
      this.loadTasks()
      document.title = this.newTaskName
    },
  }
</script>

<style>
  .new-task_form {
    margin: 24px 0px;
  }

  .tasks-list > .task {
    margin: 8px 0px;
  }
</style>
