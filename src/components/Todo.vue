<template>
  <div>
    <form @submit.prevent="add">
      <input type="hidden" v-model="input.id" />
      <input type="text" v-model="input.name" />
      <button class="btn3" type="submit" v-show="!updateSubmit">add</button>
      <button class="btn4" type="button" v-show="updateSubmit" @click="update(input)">Update</button>
    </form>
    <ol>
      <li v-for="list in todos" :key="list.id">
        <span>{{ list.name }}</span>
        <button class="btn1" @click="edit(list)">Edit</button> ||  <button class="btn2" @click="del(list)">Delete</button>
      </li>
    </ol>
  </div>
</template>


<style>
body{
  padding:5% 10% 5% 10% ;
}
.btn1, .btn2, .btn3, .btn4{
  color:white;
  border-radius: 5px;
  
}
.btn1{
  background-color: #17223b;
  border-color: transparent;
}
.btn2{
  color: black!important;
  border-color: red;
  background-color: white;
}
.btn3, .btn4{
  width: 100%;
  padding: 14px 20px;
  margin: 8px 0;
  cursor: pointer;
}
.btn3{
  background-color: #4CAF50;;
  border-color: transparent;
}
.btn4{
  background-color: #6b778d;
  border-color: transparent;
}
input[type=text], select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
</style>


<script>
import axios from "axios";

let BASE_URL = "http://localhost:3000/";

export default {
  name: "todo",
  data() {
    return {
      input: {
        id: "",
        name: ""
      },
      todos: [],
      updateSubmit: false
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios
        .get(BASE_URL + "todos")
        .then(res => {
          this.todos = res.data;
        })
        .catch(err => {
          console.log(err);
        });
    },
    add() {
      axios.post(BASE_URL + "todos/", this.input).then(res => {
        this.load();
        this.input.name = "";
      });
    },
    edit(todos){
        this.updateSubmit = true
        this.input.id = todos.id
        this.input.name = todos.name
    },
    update(input){
        return axios.put(BASE_URL+"todos/"+ input.id, { name:this.input.name }).then(res=>{
            this.load()
            this.input.id = ''
            this.input.name = ''
            this.updateSubmit = false
        }).catch((err)=>{
            console.log(err)
        })
    },
    del(todos){
        axios.delete(BASE_URL+"todos/"+ todos.id).then(res => {
            this.load()
            let index = this.todos.indexOf(input.name)
            this.todos.splice(index,1)
        })
    }
  }
};
</script>
