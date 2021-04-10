<template>
  <div id="app">
    <h1>連絡先アプリ</h1>
    <div class="new">
      <h2>新規作成</h2>
      <div class="name">
        <input type="text" name="name" id="name" v-model="newWhattodo" />
      </div>
      <button @click="insertContact">新規作成</button>
    </div>
    <div class="table">
      <table>
        <tr v-for="item in todoLists" :key="item.id">
          <td>{{ item.id }}</td>
          <td><input type="text" v-model="item.name" /></td>
          <td><input type="email" v-model="item.email" /></td>
          <td>
            <button @click="updateContact(item.id, item.name, item.email)">
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
      newWhattodo: "",
      todoLists: [],
    };
  },
  methods: {
    async getContact() {
      const resData = await axios.get("https://desolate-brook-63817.herokuapp.com/api/todos/");
      this.todoLists = resData.data.data;
    },
    async insertContact() {
      const sendData = {
        whattodo: this.newWhattodo,
      };
      await axios.post("https://desolate-brook-63817.herokuapp.com/api/todos/", sendData);
      await this.getContact();
    },
    async updateContact(id,whattodo) {
      const sendData = {
        whattodo: whattodo,
      };
      await axios.put("https://desolate-brook-63817.herokuapp.com/api/todos/" + id, sendData);
      await this.getContact();
    },
    async deleteContact(id) {
      await axios.delete("https://desolate-brook-63817.herokuapp.com/api/todos/" + id);
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