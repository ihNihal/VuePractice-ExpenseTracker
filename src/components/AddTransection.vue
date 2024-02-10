<template>
  <form id="form" @submit.prevent = "onSubmit">
    <div class="form-control">
      <label for="text">
        Text
      </label>
      <input type="text" id="text" v-model="text" placeholder="Enter Text"/>
    </div>
    <div class="form-control">
      <label for = "amount">
        Amount <br/>
        (negative - expense, positive - income)
      </label>
      <input type="text" id="number" v-model="amount" placeholder="Enter Amount" />
    </div>
    <button class="btn">Add Transection</button>
  </form>
</template>

<script setup>
import {ref} from 'vue'
import {useToast} from 'vue-toastification'

const text = ref('')
const amount = ref ('')
const emit = defineEmits(['transectionSubmitted'])


const toast = useToast()
const onSubmit = () => {
  if(!text.value || !amount.value)
  {toast.error('Both Fields must be filled');

  return
  }
  const transectionData = {
    text: text.value,
    amount: parseFloat(amount.value)


  }

  emit("transectionSubmitted", transectionData)
  text.value = '';
  amount.value = '';
}


</script>