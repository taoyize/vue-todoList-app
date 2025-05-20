<script setup>
import {ref,defineEmits} from 'vue'

const newTodo=ref('');
const emit=defineEmits(['add']);

function handleAdd(){
  if(!newTodo.value.trim()) return;
  emit('add',newTodo.value.trim());
  newTodo.value='';
}
</script>

<template>
  <div>
    <a-input v-model:value="newTodo"
    placeholder="输入任务" style="width:300px ;margin-right: 8px" />
    <a-button type="primary" @click="handleAdd">增加</a-button>
  </div>
</template>

<style scoped>

</style>
