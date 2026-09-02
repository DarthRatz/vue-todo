<template>
  <div class="container">
    <div class="row">
      <div class="col-12 py-5">
        <h1>{{ listName }}</h1>
      </div>
    </div>
    <div class="row mb-3">
      <create-todo @on-new-todo="addTodo" />
    </div>
    <div class="row">
      <div class="col-12 col-sm-10 col-lg-6">
        <ul class="list-group">
          <todo
            v-for="todo in visibleTodos"
            :key="todo.id"
            :id="todo.id"
            :description="todo.description"
            :completed="todo.completed"
            @on-toggle="toggleTodo"
            @on-delete="deleteTodo"
            @on-edit="editTodo"
          />
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import Todo from "./Todo.vue";
import CreateTodo from "./CreateTodo.vue";
export default {
  props: {
    listName: String
  },
  data() {
    return {
      todos: [
        { id: 1, description: "Do the dishes", completed: false },
        { id: 2, description: "Take out the trash", completed: false },
        { id: 3, description: "Finish doing laundry", completed: false }
      ],
      nextId: 4
    };
  },
  computed: {
    visibleTodos() {
      return this.todos;
    }
  },
  methods: {
    addTodo(newTodo) {
      if (newTodo && newTodo.length > 0) {
        this.todos.push({ id: this.nextId++, description: newTodo, completed: false });
      }
    },
    toggleTodo(id) {
      const t = this.todos.find(todo => todo.id === id);
      if (t) t.completed = !t.completed;
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    editTodo(payload) {
      const { id, description } = payload;
      const t = this.todos.find(todo => todo.id === id);
      if (t && typeof description === 'string') t.description = description;
    }
  },
  components: { Todo, CreateTodo }
};
</script>

<style scoped lang="scss"></style>