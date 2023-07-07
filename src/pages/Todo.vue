<template>
  <!-- Entradas do Usuario -->
  <q-page style="margin: 15px;">
    <p class="text-h2" style="text-align: center; margin-top: 20px;">TODO-LIST</p>
    <div style="display: flex; justify-content: center; gap: 10px;">
      <q-input outlined style="width: 150px;" v-model="newTodoText" label="Digite sua tarefa:" />
      <q-input outlined style="width: 150px;" v-model="newTodoDate" label="Data:" placeholder="00/00/0000"
        mask="##/##/####" />
      <q-btn :ripple="false" color="secondary" label="ADICIONAR" no-caps @click="addTodo" />
    </div>

    <!-- interação do usuario -->
    <div style="display: flex; justify-content: center; width: 100%;">
      <q-list style="width: 400px; margin-top: 10px;">
        <q-item v-for="(todo, index) in todos" :key="index" clickable @click="toggleCompleted(index)">
          <q-item-section>
            <template v-if="!todo.editing">
              <span :class="[{ 'text-decoration-line-through': todo.completed }]">{{ todo.text }}</span>
            </template>
            <template v-else>
              <q-input v-model="todo.text" outlined dense />
            </template>
          </q-item-section>
          <q-item-section :class="[{ 'text-decoration-line-through': todo.completed }]">{{ todo.date }}</q-item-section>

          <!-- Botoes de interação do usuario -->
          <q-item-section side>
            <q-btn icon="edit" color="info" size="sm" @click="editTodo(todo)"></q-btn>
            <q-btn icon="check" color="positive" size="sm" @click="finishEditing(todo)" style="margin-top: 10px;"
              v-if="todo.editing"></q-btn>
            <q-btn icon="delete" color="negative" size="sm" @click="deleteTodo(index)" style="margin-top: 10px;"></q-btn>
          </q-item-section>
        </q-item>
        <p v-if="todos.length === 0" style="text-align: center; margin-top: 10px; font-size: 15px; font-style: italic;">
          Não há tarefas...</p>
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
          date: this.newTodoDate,
          completed: false,
          editing: false
        }
        this.todos.push(newTodo)
        this.newTodoText = ''
        this.newTodoDate = ''
      } else if (this.newTodoText === '') {
        alert('[ERRO] Insira uma tarefa!')
      } else if (this.newTodoDate === '') {
        alert('[ERRO] Insira uma data!')
      }
    },
    editTodo (todo) {
      todo.editing = true
    },
    finishEditing (todo) {
      todo.editing = false
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

<style>
.text-decoration-line-through {
  text-decoration: line-through;
  color: #cecece;
}
</style>
