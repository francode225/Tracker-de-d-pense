<template>
  <div>
    <h4>Votre solde</h4>
    <h1 id="balance">${{ total }}</h1>
  </div>
</template>
<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps({
  total: {
    type: Number,
    required: true
  }
})
</script>
