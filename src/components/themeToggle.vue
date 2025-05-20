<script setup>
import {defineEmits ,ref,watch} from 'vue'
import {theme} from 'ant-design-vue'
const isDark=ref(localStorage.getItem("theme")==='dark')

const emit=defineEmits(['changeMode'])
const changeTheme=(checked) =>{
  isDark.value=checked;
  localStorage.setItem('theme',checked?'dark':'light');
}
watch(isDark,(val)=>{
  emit('changeMode',val?theme.darkAlgorithm:theme.defaultAlgorithm)
})
</script>

<template>
  <a-switch :checked="isDark" checked-children="🌙" un-checked-children="☀"
  @change="changeTheme"/>
</template>

<style scoped>

</style>
