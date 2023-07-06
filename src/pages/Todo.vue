<template>

  <q-page>
    <p class="text-h2" style="text-align: center; margin-top: 20px;">TODO-LIST</p>

    <div style="display: flex; justify-content: center; gap: 10px;">
      <q-input style="width: 150px;" outlined v-model="newTodoText" :dense="dense" />
      <q-input style="width: 150px;" outlined v-model="newTodoDate" placeholder="" :dense="dense" />
      <q-btn :ripple="false" color="secondary" label="ADICIONAR" no-caps @click="addTodo" />
    </div>

    <div style="display: flex; justify-content: center;">
     <q-list style="width: 490px;">
      <q-item  v-for="(todo, index) in todos" :key="index" clickable @click="toggleCompleted(index)">

        <q-item-section :style="{ 'text-decoration': todo.completed ? 'line-through' : 'none' }">{{ todo.text }}</q-item-section>
        <q-item-section>{{ todo.date }}</q-item-section>
        <q-item-section side>
          <q-btn icon="edit" color="secondary" size="sm" @click="editTodo(index)"></q-btn>
          <q-btn icon="delete" color="negative" size="sm" @click="deleteTodo(index)" style="margin-top: 10px;"></q-btn>
        </q-item-section>
      </q-item>
      <p v-if="todos.length === 0" style="text-align: center; margin-top: 10px; font-size: 15px; font-style: italic;">Não ha tarefas...</p>
     </q-list>
    </div>

  </q-page>

</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',

  data () {
    return {
      todos: [],
      newTodoText: '',
      newTodoDate: '',
      dense: true
    }
  },

  methods: {
    addTodo () {
      if (this.newTodoText.trim() !== '' && this.newTodoDate.trim() !== '') {
        const newTodo = {
          text: this.newTodoText,
          date: this.newTodoDate
        }
        this.todos.push(newTodo)
        this.newTodoText = ''
        this.newTodoDate = ''
      } else if (this.newTodoText === '') {
        alert('[ERRO] Insira um tarefa!')
      } else if (this.newTodoDate === '') {
        alert('[ERRO] Insira uma data!')
      }
    },
    editTodo (index) {
      const updatedTodoText = prompt('Editar tarefa', this.todos[index].text)
      if (updatedTodoText !== null) {
        this.todos[index].text = updatedTodoText
      }
    },
    deleteTodo (index) {
      if (confirm('Tem certeza que deseja excluir esta tarefa?')) {
        this.todos.splice(index, 1)
      }
    },
    toggleCompleted (index) {
      this.todos[index].completed = !this.todos[index].completed
    }
  }
})
</script>
