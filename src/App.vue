<template>
  <div id="app">
    <div  class="center">
    <h1>Todo List</h1>
    <div class="new">
      <div class="todo">
        <input type="text" name="name" id="name" v-model="newEvent" />
        <button @click="insertContact">追加</button>
      </div>
      
    </div>
    <div class="table">
        <div class="item" v-for="item in todoLists" :key="item.id">
          <input type="text" v-model="item.event" />
            <button @click="updateContact(item.id, item.event)">更新</button>
            <button @click="deleteContact(item.id)">削除</button>
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
    async getContact() {
      const resData = await axios.get("https://afternoon-hollows-74351.herokuapp.com/api/todos");
      this.todoLists = resData.data.data;
    },
    async insertContact() {
      const sendData = {
        event: this.newEvent,
      };
      await axios.post("https://afternoon-hollows-74351.herokuapp.com/api/todos", sendData);
      await this.getContact();
    },
    async updateContact(id,event) {
      const sendData = {
        event: event,
      };
      await axios.put("https://afternoon-hollows-74351.herokuapp.com/api/todos/" + id , sendData);
      await this.getContact();
    },
    async deleteContact(id) {
      await axios.delete("https://afternoon-hollows-74351.herokuapp.com/api/todos/" + id );
      await this.getContact();
    },
  },
  created() {
    this.getContact();
  },
};
</script>

<style>
.item{
  display: flex;
  justify-content: space-between;
}
.center{
  display: flex;
  justify-content: center;
  width:60%;
}
</style>