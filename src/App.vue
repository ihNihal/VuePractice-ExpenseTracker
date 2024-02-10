<script setup>
import { RouterLink, RouterView } from 'vue-router'

import Header from '@/components/Header.vue'
import Balance from '@/components/Balance.vue'
import IncomeExpense from '@/components/IncomeExpense.vue'
import TransectionList  from '@/components/TransectionList.vue'
import AddTransection from "@/components/AddTransection.vue";
import {ref , computed,onMounted} from 'vue'
import {useToast} from "vue-toastification";

const toast = useToast()
const transections =ref( [

])

onMounted(()=>{
  const savedTransections = JSON.parse(localStorage.getItem('transections'))
if(savedTransections){
  transections.value = savedTransections
}
})

// Get total
const total = computed(() => {
  return transections.value.reduce((acc, transection) => {
    return acc + transection.amount;
  }, 0);
});

const income = computed(() => {
  return transections.value
      .filter((transection)=> transection.amount > 0)
      .reduce((acc, transection) => {
    return acc + transection.amount;
  }, 0)
      .toFixed(2);
});

const expenses = computed(() => {
  return transections.value
      .filter((transection)=> transection.amount < 0)
      .reduce((acc, transection) => {
        return acc + transection.amount;
      }, 0)
      .toFixed(2);
});

const handleTransectionSubmitted = (transectionData)=>{
 transections.value.push({
   id: generateUniqueId(),
   text: transectionData.text,
   amount: transectionData.amount
 });
  saveTransectionsToLocalStorage();

 toast.success('Transection Added')

}
const generateUniqueId = () =>
{
  return Math.floor(Math.random() * 1000000)
}
const handleTransectionDelete = (id) => {
transections.value = transections.value.filter ((transection) =>
transection.id !== id)
  saveTransectionsToLocalStorage;
  toast.error('Transection Deleted')
}

const saveTransectionsToLocalStorage = ()=>{
   localStorage.setItem('transections', JSON.stringify(transections.value))
}


</script>

<template>
 
<Header/>

  <div class = "container">
    <Balance :total="+total"/>
    <income-expense :income="income" :expenses="expenses"/>
    <TransectionList :transections="transections" @transectionDeleted="handleTransectionDelete"/>
    <AddTransection @transectionSubmitted="handleTransectionSubmitted"/>
  </div>
  
</template>

<style scoped>

</style>
