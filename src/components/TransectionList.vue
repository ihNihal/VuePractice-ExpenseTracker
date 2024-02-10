<template>
  <h3>History</h3>

  <ul id="list" class="list">
    <li
    v-for="transection in transections"
    :key="transection.id"
    :class="transection.amount < 0 ? 'minus': 'plus'"
    >
      {{ transection.text }} <span>${{transection.amount}}</span><button @click = "deleteTransection(transection.id)" class = "delete-btn">x</button>
    </li>
  </ul>

</template>
<script setup>

import {defineProps} from 'vue'
const props = defineProps({
  transections: {
    type: Array,
    required: true
  }
})
const emit = defineEmits('transectionDeleted')

const deleteTransection = (id)=>{
  emit('transectionDeleted', id)
}
</script>