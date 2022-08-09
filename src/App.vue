<template>
  <div class="container">
    <div class="add-list">
      <input v-model="value" id="search" />
      <button @click="todo">Добавить задачу</button>
    </div>
    <div v-if="innerData.activeFilter == ''">
      <div v-for="todo in innerData.zadachi" :key="todo.id" class="todo-item">
        <input v-model="todo.active" type="checkbox" />
        <span v-if="todo.active" style="text-decoration: line-through">
          {{ todo.text }}
        </span>
        <span v-else-if="!todo.active">
          {{ todo.text }}
        </span>
        <button @click="remove(todo)">удалить</button>
      </div>
    </div>

    <div v-else-if="innerData.activeFilter == 'all'">
      <div v-for="todo in innerData.zadachi" :key="todo.id" class="todo-item">
        <input v-model="todo.active" type="checkbox" />
        <span v-if="todo.active" style="text-decoration: line-through">
          {{ todo.text }}
        </span>
        <span v-else-if="!todo.active">
          {{ todo.text }}
        </span>
        <button @click="remove(todo)">удалить</button>
      </div>
    </div>

    <div v-else-if="innerData.activeFilter == 'active'">
      <div v-for="todo in innerData.zadachi" :key="todo.id">
        <div v-if="todo.active != true" class="todo-item">
          <input v-model="todo.active" type="checkbox" />
          {{ todo.text }}
          <button @click="remove(todo)">удалить</button>
        </div>
      </div>
    </div>

    <div v-else-if="innerData.activeFilter == 'completed'">
      <div v-for="todo in innerData.zadachi" :key="todo.id">
        <div v-if="todo.active == true" class="todo-item">
          <input v-model="todo.active" type="checkbox" />
          <span style="text-decoration: line-through">
            {{ todo.text }}
          </span>
          <button @click="remove(todo)">удалить</button>
        </div>
      </div>
    </div>

    <div>
      <span class="button button-all" @click="setFilter('all')">Все</span>
      <span class="button button-active" @click="setFilter('active')"
        >Активные</span
      >
      <span class="button button-completed" @click="setFilter('completed')"
        >Завершенные</span
      >
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      innerData: {
        zadachi: [],
        activeFilter: "",
      },
      value: "",
    };
  },
  methods: {
    todo() {
      this.innerData.zadachi.push({
        text: this.value,
        id: Date.now(),
        active: false,
      });
      this.value = "";
    },
    remove(todo) {
      this.innerData.zadachi = this.innerData.zadachi.filter(
        (t) => t.id !== todo.id
      );
    },

    setFilter(filter) {
      this.innerData.activeFilter = filter;
    },
  },
  async mounted() {
    await axios
      .get("https://my-json-server.typicode.com/falk20/demo/todos")
      .then((response) => {
        this.innerData.zadachi = response.data;
      });
  },
};
</script>

<style>
.container {
  max-width: 90%;
  display: block;
  margin: auto;
}
.add-list {
  width: 100%;
  margin: 20px 0;
  display: flex;
}
.add-list input {
  box-sizing: border-box;
  height: 40px;
  border: 1px solid;
  width: 100%;
  padding: 5px 15px;
}
.add-list button {
  height: 40px;
  border: 1px solid;
  background: #fff;
  margin-left: 10px;
  cursor: pointer;
}
.button {
  margin: 20px;
  cursor: pointer;
  border: 1px solid;
  padding: 5px 10px;
  display: block;
  text-align: center;
}
.button-completed {
  background: #ff0000;
}
.button-all {
  background: #0833e7;
}
.button-active {
  background: #58ec04;
}
.todo-item {
  display: flex;
  margin-bottom: 10px;
  align-items: center;
}
.todo-item input {
  margin-right: 10px;
}
.todo-item button {
  box-sizing: border-box;
  background: red;
  color: #fff;
  padding: 5px 15px;
  margin-left: 10px;
  border: 0;
}
</style>
