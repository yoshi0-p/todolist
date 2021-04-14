<template>
<div id="app" >
      <div class="List">
        <h2> Todo List</h2>  
        <div class="forms">
          <div class="new">
          <input class="text long" type="text"  v-model="newEvent" />
          <button class="btn ins" @click= "insertTodo">追加</button>
          </div>
          <div class="item" v-for="item in todoLists" :key="item.id">
            <input class="text" type="text" v-model="item.event" />
            <div class="menu">
            <button class ="btn up" @click= "updateTodo(item.id, item.event)">更新</button>
            <button class ="btn del" @click= "deleteTodo(item.id)">削除</button>
            </div>
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
      this.newEvent = "";
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
html{
  background-color:#2D197C;
}
#app{
  display: flex;
  justify-content: center;
  padding:10%;
}
.List{
  width:60%;
  height:50%;
  background-color:white;
  padding:1%;
  border-radius:10px;
}
.new{
  display: flex;
  justify-content:space-between;
  margin-bottom: 20px;
}
.item{
  display: flex;
  justify-content:space-between;
}
.btn{
  width:70px;
  height:40px;
  background-color:white;
  border-radius: 5px;
  margin-right:2px;
}
.btn:hover{
  color:white;
}
.ins{
border-color:#DF7BFB;
color:#DF7BFB;
}
.ins:hover{
background-color:#DF7BFB;
}
.del{
border-color:#34ff9a;
color:#34ff9a;
}
.del:hover{
background-color:#34ff9a;
}
.up{
border-color:#fbbf7b;
color:#fbbf7b;
}
.up:hover{
background-color:#fbbf7b;
}

.menu{
  padding-bottom:10px;
}
.text{
  height:40px;
  border-radius: 5px;
  border:1px solid silver;
  border-width:1px;
}
.long{
  width:80%;
}


</style>