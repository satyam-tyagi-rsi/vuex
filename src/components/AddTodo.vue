<template>
  <div>
    <h3>{{header}}</h3>
    <div class="add">
      <form @submit="onSubmit">
        <input type="text" v-model="todoObject.title" placeholder="Add Todo...">
        <input v-if="isUpdateTodo" type="button" value="Cancel" @click="resetTodo">
        <input type="submit" value="Submit">
      </form>
    </div>
  </div>
</template>

<script>
import { mapActions } from "vuex";
export default {
  name: "AddTodo",
  data() {
    return {
      header: "Add Todo",
      isUpdateTodo: false,
      todoObject: {
        title: ""
      }
    };
  },
  methods: {
    ...mapActions(["addTodo", "updateTodo"]),
    onSubmit(e) {
      e.preventDefault();
      if(this.todoObject.id) {
        this.updateTodo(this.todoObject);
      } else {
        this.addTodo(this.todoObject.title);
      }
      this.resetTodo();
    },
    resetTodo() {
      this.isUpdateTodo = false;
      this.header = "Add Todo";
      this.todoObject = {};
      this.todoObject.title = "";
    }
  },
  mounted() {
    this.$root.$on('edit-todo', editTodo => {
        if(editTodo) {
          this.todoObject = editTodo;
          this.isUpdateTodo = true;
          this.header = `Update Todo: ${editTodo.id}`;
        }
    });
  }
};
</script>

<style scoped>
form {
  display: flex;
}

input[type="text"] {
  flex: 10;
  padding: 10px;
  border: 1px solid #41b883;
  outline: 0;
}

input[type="submit"] {
  flex: 2;
  background: #41b883;
  color: #fff;
  border: 1px #41b883 solid;
  cursor: pointer;
}

input[type="button"] {
  flex: 2;
  background: red;
  color: #fff;
  border: 1px red solid;
  cursor: pointer;
}
</style>
