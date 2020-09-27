<template>
  <div id="app">
    <h1 class="title">{{ title }}</h1>
    <div class="todo">
      <input v-model="todo" type="text" placeholder="type todo..." />
      <button @click="addTodo(id++)">add todo</button>
      <p></p>
      <!-- <p>{{ todo }}</p> -->
      <div v-for="(todo, i) in todos" :key="todo.id">
        <p>
          <span class="todo__id">{{ 1 + i + ")" }}</span>
          <span
            class="todo__text"
            :class="{todo__text_isShow: todo.isComplete}"
            @dblclick="removeTodo(i)"
          >{{" " + todo.text }}</span>
          <input v-model="todo.isComplete" class="todo__check" type="checkbox" />
        </p>
      </div>
      <hr />
      <p>Список задач: {{ todos.length }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "Todo app",
      todo: "",
      todos: [],
      isDone: false,
      id: 0,
    };
  },
  async mounted() {
    const data = await localStorage.getItem("todos");
    data ? (this.todos = JSON.parse(data)) : null;
  },
  methods: {
    addTodo() {
      if (this.todo != "" && this.todo.length < 10) {
        this.todos.push({
          id: this.id,
          text: this.todo,
          isComplete: this.isDone,
        });
        localStorage.setItem("todos", JSON.stringify(this.todos));
      }
      this.todo = "";
    },
    removeTodo(index) {
      console.log(index);
      this.todos.splice(index, 1);
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
};
</script>

<style>
#app {
}
.title {
  text-align: center;
}
.todo {
  width: 300px;
  height: 100%;
  background-color: rgb(208, 238, 206);
  padding: 30px;
  outline: 1px solid blue;
  min-height: 500px;
}
.todo__id {
  color: red;
  font-weight: bold;
}
.todo__text {
  font-size: 15px;
  color: #499;
  text-transform: capitalize;
}
.todo__text_isShow {
  color: rgb(0, 0, 0);
  text-decoration: line-through;
}
.todo__check {
  display: inline-block;
  margin-left: 10px;
}
</style>
