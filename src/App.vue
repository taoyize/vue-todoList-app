<script setup>
import {ref,computed,watch} from 'vue'
import {theme} from 'ant-design-vue'
import TodoInput from "@/components/TodoInput.vue";
import todoList from "@/components/todoList.vue"
import ThemeToggle from "@/components/themeToggle.vue";
const saveTodos=localStorage.getItem('todos')
const todos=ref(saveTodos?JSON.parse(saveTodos):[]);
const currentTheme=ref(localStorage.getItem('theme')==='dark'?
theme.darkAlgorithm:theme.defaultAlgorithm)
function addTodo(text){
  todos.value.push({text,completed:false});
}

function toggleCompleted(index){
  todos.value[index].completed=!todos.value[index].completed;
}

function removeTodo(index){
  todos.value.splice(index,1);
}

const completedCount = computed(() => todos.value.filter(t => t.completed).length)

watch(todos,(newTodo)=>{
  localStorage.setItem('todos',JSON.stringify(newTodo))
},{deep:true})

</script>

<template>
  <a-config-provider :theme="{ algorithm: currentTheme }">
    <themeToggle @changeMode="val=>currentTheme=val"/>
    <a-typography-title style="margin-top: 20px;text-align: center">todo-list</a-typography-title>
    <div style="padding: 24px; max-width: 480px; margin: auto;">

      <TodoInput @add="addTodo" />
      <todoList
        :todos="todos"
        @toggle="toggleCompleted"
        @remove="removeTodo"
      />
      <div style="margin-top: 16px;">
        已完成任务：{{ completedCount }}
      </div>
    </div>
  </a-config-provider>

</template>


<style scoped>

input[type="text"] {
  flex: 1;
  padding: 8px;
  font-size: 14px;
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

</style>
