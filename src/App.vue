<template>
<div id="app" >
  <div  class="center">
      <div>
      <h1>Todo List</h1>
      <div class="todo">
        <input type="text" name="name" id="name" v-model="newEvent" />
        <button @click= "insertTodo">追加</button>
      </div>
      <div class="item" v-for="item in todoLists" :key="item.id">
       <input type="text" v-model="item.event" />
       <button @click= "updateTodo(item.id, item.event)">更新</button>
       <button @click= "deleteTodo(item.id)">削除</button>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      newEvent: "",
      todoLists: [],
    };
  },
  methods: {
    async getTodo() {
      const resData = await axios.get("https://afternoon-hollows-74351.herokuapp.com/api/todos");
      this.todoLists = resData.data.data;
    },
    async insertTodo() {
      const sendData = {
        event: this.newEvent,
      };
      await axios.post("https://afternoon-hollows-74351.herokuapp.com/api/todos", sendData);
      await this.getTodo();
    },
    async updateTodo(id,event) {
      const sendData = {
        event: event,
      };
      await axios.put("https://afternoon-hollows-74351.herokuapp.com/api/todos/" + id , sendData);
      await this.getTodo();
    },
    async deleteTodo(id) {
      await axios.delete("https://afternoon-hollows-74351.herokuapp.com/api/todos/" + id );
      await this.getTodo();
    },
  },
  created() {
    this.getTodo();
  },
};
</script>

<style>
#app{
  background-color:purple;
}
.item{
  display: flex;
  justify-content: space-between;
}
.center{
  display: flex;
  justify-content: center;
  width:60%;
  margin-top:20%;
  background-color:white;
}
</style>