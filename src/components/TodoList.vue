<template>
  <div class="container pt-3">
    <div class="row justify-content-around mb-3">
      <div class="col-md-4">
        <button @click="showField = !showField" type="button" class="btn btn-success mr-1">Создать новый лист</button>
        <button @click="clearTodoList" type="button" class="btn btn-danger">Удалить все</button>
      </div>
      <div class="input-group col-md-4">
        <input v-model.trim="searchTodo" type="text" class="form-control" placeholder="Search for..." aria-label="Recipient's username" aria-describedby="button-addon2">
        <div class="input-group-append">
          <button class="btn btn-outline-secondary" type="button" id="button-addon2">Go!</button>
        </div>
      </div>
    </div>
    <div v-show="showField" class="row justify-content-center mb-2">
      <form class="col-md-4" @submit.prevent="createTodo">
        <div class="input-group">
          <input v-model="todoTitle" type="text" class="form-control" placeholder="описание..." aria-label="Recipient's username" aria-describedby="button-addon2">
          <div class="input-group-append">
            <button class="btn btn-outline-secondary" type="submit" id="button-addon2">Ок!</button>
          </div>
        </div>
      </form>
    </div>
    <div v-for="(item, index) in getTodoList" :key="item.id">
      <div class="shadow-lg p-3 mb-2 bg-white rounded">
        <div class="row align-items-center pl-1">
          <div v-if="item.isChange" class="mr-1">
            <input v-model.trim="item.name" type="text" class="form-control">
          </div>
          <div v-else class="mr-1">{{ item.name }}</div>
          <div v-if="item.isChange">
            <button @click="saveNameTodo(item)" type="button" class="btn btn-success mr-1">Сохранить</button>
            <button @click="cancelChange(item, index)" type="button" class="btn btn-danger">Отмена</button>
          </div>
          <div v-else>
            <button @click="changeNameTodo(item)" type="button" class="btn btn-warning mr-1 text-white">Редактировать</button>
            <button @click="deleteTodo(item)" type="button" class="btn btn-danger">Удалить</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data() {
    return {
      showField: false,
      todoTitle: '',
      searchTodo: '',
      todoList: [],
      todoListCopy: []
    }
  },
  computed: {
    getTodoList () {
      return this.todoList.filter(item => {
        return item.name.toLowerCase().includes(this.searchTodo.toLowerCase())
      })
    }
  },
  methods: {
    createTodo () {
      if (this.todoTitle !== '') {
        this.todoList.push({
          name: this.todoTitle,
          id: Math.random(),
          isChange: false
        })
        this.showField = false
        this.todoTitle = ''
        this.todoListCopy = JSON.parse(JSON.stringify(this.todoList))
      } else {
        alert('Введите название')
      }
    },
    deleteTodo (item) {
      this.todoList.splice(this.todoList.indexOf(item), 1)
    },
    clearTodoList () {
      this.todoList = []
    },
    changeNameTodo (item) {
      item.isChange = true
    },
    cancelChange (item, index) {
      this.todoList[index].name = this.todoListCopy[index].name
      item.isChange = false
    },
    saveNameTodo (item) {
      if (item.name !== '') {
        item.isChange = false
      } else {
        alert('Поле не может быть пустым')
      }
    }
  }
}
</script>

<style scoped>
</style>
