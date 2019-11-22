/* eslint-disable no-return-assign */
<template>
  <div>
    <input
      type="text"
      class="form-textarea w-full my-4"
      placeholder="what needs to be done?"
      v-model="newTodo"
      @keyup.enter="addTodo"
    >
    <div
      v-for="(todo, index) in todos"
      :key="todo.id"
    >
      <div class="flex my-2">
        <!-- Toggle Todo Status -->
        <input
          type="checkbox"
          v-model="todo.completed"
          class="form-checkbox mr-2"
        >
        <!-- Todo Label -->
        <div
          v-if="!todo.editing"
          @dblclick="editTodo(todo)"
          class="leading-none"
          :class="{ 'line-through text-gray-500' : todo.completed }"
        >
          {{ todo.title }}
        </div>
        <!-- Edit Todo Form -->
        <input
          type="text"
          v-model="todo.title"
          v-else
          class="form-textarea"
          @blur="doneEdit(todo)"
          @keyup.enter="doneEdit(todo)"
          v-focus
          @keyup.esc="cancelEdit(todo)"
        >
        <!-- Remove Todo -->
        <div
          class="remove-item cursor-pointer hover:text-green-700
          justify-end ml-auto text-lg my-auto"
          @click="removeTodo(index)"
        >
          &times;
        </div>
      </div>
    </div>
    <div>
      <div class="flex border rounded p-4 border-gray-300">
        <!-- Complete All -->
        <input
          type="checkbox"
          name="check-all"
          id="check-all"
          class="form-checkbox leading-none mr-2"
          :checked="!anyRemaining"
          @change="checkAllTodos"
        >
        <label
          for="check-all"
          class="leading-none"
        >
          Complete All
        </label>
        <!-- Num Remaining -->
        <div class="leading-none ml-auto ">
          {{ remaining }} items left
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data() {
    return {
      newTodo: '',
      idForTodo: 3,
      beforeEditCache: '',
      todos: [{
        id: 1,
        title: 'Finish vue screencast',
        completed: false,
        editing: false,
      }, {
        id: 2,
        title: 'Take over world',
        completed: false,
        editing: false,
      }],
    };
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length;
    },
    anyRemaining() {
      return this.remaining !== 0;
    },
  },
  directives: {
    focus: {
      inserted(el) {
        el.focus();
      },
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length === 0) {
        return;
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
        editing: false,
      });

      this.newTodo = '';
      this.idForTodo += 1;
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title;
      // eslint-disable-next-line no-param-reassign
      todo.editing = true;
    },
    doneEdit(todo) {
      console.log(todo.title.trim());
      if (todo.title.trim() === '') {
        // eslint-disable-next-line no-param-reassign
        todo.title = this.beforeEditCache;
      }
      // eslint-disable-next-line no-param-reassign
      todo.editing = false;
    },
    cancelEdit(todo) {
      // eslint-disable-next-line no-param-reassign
      todo.editing = false;
      // eslint-disable-next-line no-param-reassign
      todo.title = this.beforeEditCache;
    },
    checkAllTodos() {
      this.todos.forEach((todo) => {
        todo.completed = event.target.checked;
      });
    },
  },
};
</script>
