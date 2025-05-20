<script setup>
import {defineEmits,defineProps} from 'vue'
import todoItem from './todoItem.vue'

const props=defineProps({
  todos:Array
});

const emit=defineEmits(['toggle','remove'])

function toggleCompleted(index){
  emit('toggle',index);
}

function removeTodo(index){
  emit('remove',index);
}
</script>

<template>
  <a-list :data-source="props.todos">
    <template #renderItem="{ item, index }">
      <todoItem
        :todo="item"
        :index="index"
        @toggle="toggleCompleted"
        @remove="removeTodo"
      />
    </template>
  </a-list>

</template>

<style scoped>

</style>
