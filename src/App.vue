<template>
  <div class="container">
    <div class="container__item">
      <form class="form">
        <input
          v-model="todo.name"
          type="text"
          class="form__field"
          placeholder="Add Your Todo"
        />
        <button
          @click="addTodo()"
          type="button"
          class="btn btn--primary btn--inside uppercase"
        >
          Add Todo
        </button>
      </form>
    </div>
    <ul v-for="(todo, index) in todos" :key="index">
      <li @click="changeTodoStatus(todo)">
        <span :class="todo.completed ? 'todo__completed' : ''">{{
          todo.name
        }}</span>
        <span @click="removeTodo(todo)"> &#10007;</span>
      </li>
    </ul>
  </div>
</template>

<script>
import {
  getFirestore,
  onSnapshot,
  setDoc,
  doc,
  deleteDoc,
} from "firebase/firestore";
export default {
  name: "App",
  data() {
    return {
      todo: {
        name: null,
        completed: false,
      },
      todos: [],
      db: getFirestore(),
    };
  },
  methods: {
    async addTodo() {
      this.todos.push(this.todo);
      await setDoc(doc(this.db, "todosCollection", "todos"), {
        ...this.todos,
      });
      this.todo.name = null;
    },
    async removeTodo(payload) {
      let updatedTodos = [...this.todos].filter(
        (item) => item.name !== payload.name
      );
      await deleteDoc(doc(this.db, "todosCollection", "todos"));
      await setDoc(doc(this.db, "todosCollection", "todos"), {
        ...updatedTodos,
      });
    },
    async changeTodoStatus(payload) {
      let updatedTodos = [...this.todos].map((item) => {
        if (item.name === payload.name) {
          item.completed = true;
        }
        return item;
      });
      await setDoc(doc(this.db, "todosCollection", "todos"), {
        ...updatedTodos,
      });
    },
    getTodos() {
      onSnapshot(doc(this.db, "todosCollection", "todos"), (doc) => {
        const arr = doc.data();
        if (arr) {
          this.todos = Object.values(arr);
        }
      });
    },
  },
  created() {
    this.getTodos();
  },
};
</script>

<style lang="scss">
//** variables
$background: #f5f6fa;
$text: #9c9c9c;
$input-bg-color: #fff;
$input-text-color: #a3a3a3;
$button-bg-color: #7f8ff4;
$button-text-color: #fff;

//** root
:root {
  background: $background;
  color: $text;
  font: 1rem "PT Sans", sans-serif;
}

a {
  color: inherit;

  &:hover {
    color: $button-bg-color;
  }
}

//** helper
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: auto;
  width: 30%;
  padding: 3rem;
  margin: auto;
  background: white;
  border-radius: 10px;
  -webkit-box-shadow: 0 10px 6px -6px #777;
  -moz-box-shadow: 0 10px 6px -6px #777;
  box-shadow: 0 13px 11px 1px #777;
}

.uppercase {
  text-transform: uppercase;
}

//** button
.btn {
  display: inline-block;
  background: transparent;
  color: inherit;
  font: inherit;
  border: 0;
  outline: 0;
  padding: 0;
  transition: all 200ms ease-in;
  cursor: pointer;

  &--primary {
    background: $button-bg-color;
    color: $button-text-color;
    box-shadow: 0 0 10px 2px rgba(0, 0, 0, 0.1);
    border-radius: 2px;
    padding: 12px 36px;

    &:hover {
      background: darken($button-bg-color, 4%);
    }

    &:active {
      background: $button-bg-color;
      box-shadow: inset 0 0 10px 2px rgba(0, 0, 0, 0.2);
    }
  }

  &--inside {
    margin-left: -96px;
  }
}

//** form
.form {
  &__field {
    width: 360px;
    background: #fff;
    color: $input-text-color;
    font: inherit;
    box-shadow: 0 6px 10px 0 rgba(0, 0, 0, 0.1);
    border: 0;
    outline: 0;
    padding: 22px 18px;
  }
}

ul {
  list-style: none;
  width: 100%;
  cursor: pointer;
  margin: 5px;
  padding: 1rem;
  background: #fff;
  box-shadow: 0 6px 10px 0 rgba(0, 0, 0, 0.1);

  li {
    display: flex;
    justify-content: space-between;
  }
}
.todo__completed {
  text-decoration: line-through;
}
</style>
