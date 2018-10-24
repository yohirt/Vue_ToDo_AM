<template>
  <div class="hello">
      <input type="text" class="todo-input" placeholder="Co chciałbyć zrobić?" v-model="newTodo" @keyup.enter="addTodo">
      <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item" >
        <div class="todo-item-left">
          <input type="checkbox" v-model="todo.completed">
          <div v-if="!todo.editing" class="todo-item-label" @dblclick="editTodo(todo)" :class="{completed : todo.completed}" >
            {{ todo.title}}
          </div>
        <input v-else type="text" v-model="todo.title" class="todo-item-edit" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
          
        </div>
        <div class="remove-item" @click="removeTodo(index)">
          &times; 
        </div>
      </div>
  </div>
</template>

<script>
export default {
  name: "todo-list",
  data() {
    return {
      newTodo: "",
      idForTodo: 3,
      beforeEditCasche: "",
      todos: [
        {
          id: 1,
          title: "Finish Vue Scrncast",
          completed: false,
          editing: false
        },
        {
          id: 2,
          title: "Take over world",
          completed: false,
          editing: false
        }
      ]
    };
  },
  directives: {
    focus: {
      // directive definition
      inserted: function(el) {
        el.focus();
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return;
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false
      });

      this.newTodo = "";
      this.idForTodo++;
    },

    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    editTodo(todo) {
      this.beforeEditCasche = todo.title;
      todo.editing = true;
    },
    doneEdit(todo) {
      if (todo.title.trim() == "") {
        todo.title = this.beforeEditCasche;
      }
      todo.editing = false;
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCasche;
      todo.editing = false;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.todo-item-edit {
  font-size: 24px;
  color: red;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #000;
  font-family: "Anevir", Helvetica, Arial, sans-serif;
}
.remove-item {
  cursor: pointer;
  margin-left: 14px;
  // font-size: 2px;
  &:hover {
    color: red;
  }
}
.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus {
    outline: 0;
  }
}
.todo-item-label, .todo-item-edit{
  display: inline-block;
  width: auto;
}
.completed{
  text-decoration: line-through;
  color: silver;
}

</style>
 