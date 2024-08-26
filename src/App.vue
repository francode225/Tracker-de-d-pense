<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+debit" :expense="+credit" />
    <TransactionList :transactions="transactions" @transactionSupprime="transactionSupprime" />
    <AddTransaction @transactionSubmitted="ajoutTransaction" />
  </div>
</template>
<script setup lang="ts">
import { useToast } from 'vue-toastification'
import Header from './components/Header.vue'
import Balance from './components/balance.vue'
import IncomeExpense from './components/IncomeExpense.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'

import { ref, computed, onMounted } from 'vue'

onMounted(() => {
  const savedTransactions = localStorage.getItem('transactions')
  const enregistrerTransactions = savedTransactions ? JSON.parse(savedTransactions) : []

  if (enregistrerTransactions) {
    transactions.value = enregistrerTransactions
  }
})
interface Transaction {
  id: number
  amount: string
  text: string
}
const transactions = ref<Transaction[]>([])

const toast = useToast()

const total = computed(() => {
  return transactions.value.reduce((accumulateur: number, transaction) => {
    return accumulateur + parseFloat(transaction.amount)
  }, 0)
})

// ma methode sans filter
// const debit = computed(() => {
//   return transactions.value.reduce((accumulateur: number, transaction) => {
//     return accumulateur + (transaction.amount > 0 ? transaction.amount : 0)
//   }, 0)
// })

// avec filter
const debit = computed(() => {
  return transactions.value
    .filter((transaction) => parseFloat(transaction.amount) > 0)
    .reduce((accumulateur: number, transaction) => {
      return accumulateur + parseFloat(transaction.amount)
    }, 0)
  //.toFixed(2)  pour avoir 2 chiffres apres la virgule
})

const credit = computed(() => {
  return transactions.value
    .filter((transaction) => parseFloat(transaction.amount) < 0)
    .reduce((accumulateur: number, transaction) => {
      return accumulateur + parseFloat(transaction.amount)
    }, 0)
  // .toFixed(2)
})

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}

interface TransactionData {
  text: string
  amount: string
}
const ajoutTransaction = (transactionData: TransactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  })
  enregistrerToLoacalStorage()

  toast.success('transcation effectuée avec succès')
}

const transactionSupprime = (id: number) => {
  const confirmation = window.confirm('voulez-vous vraiment supprimer cette transaction ?')
  if (confirmation) {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
    toast.success('transaction supprimée avec succès')
  }
}

/* 
une deuxieme fonction pour supprimer directement au lieu de filtrer le tableau
const transactionSupprime = (id: number) => {
  const confirmation = window.confirm('Voulez-vous vraiment supprimer cette transaction ?');
  if (confirmation) {
    const index = transactions.value.findIndex((transaction) => transaction.id === id);
    if (index !== -1) {
      transactions.value.splice(index, 1);
      toast.success('Transaction supprimée avec succès');
    } else {
      toast.error('Transaction introuvable');
    }
  }
};
*/

const enregistrerToLoacalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>
