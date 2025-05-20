<script setup>
import {defineProps,defineEmits} from 'vue'

const props=defineProps({
  todo: Object,
  index: Number,
})

const emit=defineEmits(['toggle','remove'])

function toggleComplete(){
  emit('toggle',props.index);
}

function remove(){
  emit('remove',props.index);
}
</script>

<template>
  <a-list-item>
    <a-checkbox @change="toggleComplete"/>
    <span :style="{ textDecoration: todo.completed ? 'line-through' : 'none', marginLeft: '8px' }">
      {{todo.text}}
    </span>
    <a-button type="text" danger @click="remove" style="float: right">删除</a-button>
  </a-list-item>
</template>

<style scoped>

</style>
