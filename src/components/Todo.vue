<template>
  <li class="d-flex align-items-center list-group-item">
    <button
      class="btn border-0 flex-grow-1 text-left shadow-none"
      :class="{ completed }"
      @click="$emit('on-toggle', id)"
      v-if="!isEditing"
    >
      <span>{{ description }}</span>
    </button>
    <form v-else class="flex-grow-1" @submit.prevent="finishEditing()">
      <input
        type="text"
        class="form-control"
        v-model="newTodoDescription"
        @blur="finishEditing()"
        ref="newTodo"
      />
    </form>
    <button @click="startEditing()" class="btn btn-outline-primary border-0 ml-2">
      <span class="fa fa-edit"></span>
    </button>
    <button @click="$emit('on-delete', id)" class="btn btn-outline-danger border-0">
      <span class="fa fa-trash"></span>
    </button>
  </li>
</template>

<script>
export default {
  name: "TodoItem",
  data() {
    return {
      isEditing: false,
      newTodoDescription: ""
    };
  },
  props: {
    id: Number,
    description: String,
    completed: Boolean
  },
  methods: {
    startEditing() {
      if (this.isEditing) {
        this.finishEditing();
      } else {
        this.newTodoDescription = this.description;
        this.isEditing = true;
        this.$nextTick(() => this.$refs.newTodo.focus());
      }
    },
    finishEditing() {
      this.isEditing = false;
      this.$emit("on-edit", { id: this.id, description: this.newTodoDescription });
    }
  }
};
</script>

<style lang="scss" scoped>
.completed {
  text-decoration: line-through;
}
</style>
