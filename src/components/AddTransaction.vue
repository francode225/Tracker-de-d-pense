<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" v-model="text" id="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount">Amount <br /> </label>
      <input type="text" v-model="amount" id="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
<script setup lang="ts">
import { ref } from 'vue'
import { useToast } from 'vue-toastification'
const toast = useToast()
const text = ref<string>('')

const amount = ref<string>('')

const emit = defineEmits(['transactionSubmitted'])
const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('Remplissez tous les champs')
    return
  }

  const transactionData = {
    text: text.value,
    amount: amount.value
  }

  emit('transactionSubmitted', transactionData)

  text.value = ''
  amount.value = ''
}
</script>
