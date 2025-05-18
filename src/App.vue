<script setup>
import {ref,computed,watch} from 'vue'
import {theme} from 'ant-design-vue'
const saveTodos=localStorage.getItem('todos')
const todos=ref(saveTodos?JSON.parse(saveTodos):[]);
const newTodo=ref('');
const isDark=ref(false);
function addTodo(){
  if(newTodo.value.trim()==='') return;
  todos.value.push({
    text:newTodo.value.trim(),
    completed:false,
  })
  newTodo.value='';
}

function deleteTodo(index){
  todos.value.splice(index,1);
}

const completeCount=computed(()=>
  todos.value.filter(todo=>todo.completed).length
)

watch(todos,(newTodo)=>{
  localStorage.setItem('todos',JSON.stringify(newTodo))
},{deep:true})

function switchMode(){
  isDark.value=!isDark.value;
}
</script>

<template>
  <a-config-provider :theme="{ algorithm: isDark ? theme.darkAlgorithm : theme.defaultAlgorithm }">
    <a-button type="link" @click="switchMode">切换模式</a-button>
    <a-typography class="container">
      <a-typography-title>todolist</a-typography-title>
      <div class="inputArea">
        <a-input
          v-model:value="newTodo"
          placeholder="new todo"
          show-count :maxlength="15"/>

        <a-button type="primary" @click="addTodo">添加</a-button>
      </div>
      <ul class="todoList">
        <li v-for="(item,index) of todos" :key="index" :class="{done:item.completed}">
          <a-checkbox v-model:checked="item.completed">完成</a-checkbox>
          <span>{{item.text}}</span>
          <a-button @click="deleteTodo(index)">删除</a-button>
        </li>
      </ul>
      <p class="status">
        completed:{{completeCount}} / {{todos.length}}
      </p>
    </a-typography>
  </a-config-provider>

</template>


<style scoped>
.container {
  max-width: 80%;
  margin: 50px auto;
  padding: 20px;
  font-family: 'Helvetica Neue', sans-serif;
  border: 1px solid #ddd;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.05);
  background: linear-gradient(to right,white,grey);
}

h1 {
  text-align: center;
  color: #333;
}
.inputArea {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 8px;
  font-size: 14px;
}

.todoList {
  list-style: none;
  padding: 0;
}

.todoList li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 6px 0;
  border-bottom: 1px solid #eee;
}

.todoList li.done span {
  text-decoration: line-through;
  color: #888;
}

.status {
  text-align: center;
  margin-top: 15px;
  font-size: 20px;
  color: #666;
}
</style>
