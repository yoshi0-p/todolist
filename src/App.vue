<template>
  <div id="app">
    <h1>連絡先アプリ</h1>
    <div class="new">
      <h2>新規作成</h2>
      <div class="name">
        <input type="text" name="name" id="name" v-model="newEvent" />
      </div>
      <button @click="insertContact">新規作成</button>
    </div>
    <div class="table">
      <table>
        <tr v-for="item in todoLists" :key="item.id">
          <td>{{ item.id }}</td>
          <td><input type="text" v-model="item.event" /></td>
          <td>
            <button @click="updateContact(item.id, item.event)">
              更新
            </button>
          </td>
          <td>
            <button @click="deleteContact(item.id)">削除</button>
          </td>
        </tr>
      </table>
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
table,
td,
th {
  border: 1px solid #000;
  border-collapse: collapse;
  text-align: center;
}
td,
th {
  padding: 5px;
}
th {
  background: #f0e6cc;
}
</style>