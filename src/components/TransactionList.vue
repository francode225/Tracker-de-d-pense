<template>
  <h3>Historique</h3>
  <ul class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="parseFloat(transaction.amount) < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }}
      <span>${{ transaction.amount }}</span>
      <button @click="supprimerTransaction(transaction.id)" class="delete-btn">X</button>
    </li>
  </ul>
</template>
<script setup lang="ts">
import { defineProps } from 'vue'
interface Transaction {
  id: number
  amount: string
  text: string
}

const emit = defineEmits(['transactionSupprime'])

const supprimerTransaction = (id: number) => {
  emit('transactionSupprime', id)
}

const props = defineProps<{ transactions: Transaction[] }>()
</script>
