<template>
  <div class="app">
    <div class="container">
      <!-- <h1 class="app-title">Expense Tracker</h1> -->
      <AddTransaction @addTransaction="addTransaction" />
      <TransactionList :transactions="transactions" :deleteTransaction="deleteTransaction" />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';
import AddTransaction from './components/AddTransaction.vue';
import TransactionList from './components/TransactionList.vue';

const transactions = ref([]);

// Load from localStorage on mount
onMounted(() => {
  const savedTransactions = localStorage.getItem('transactions');
  if (savedTransactions) {
    transactions.value = JSON.parse(savedTransactions);
  }
});

// Save to localStorage whenever transactions change
watch(transactions, (newTransactions) => {
  localStorage.setItem('transactions', JSON.stringify(newTransactions));
}, { deep: true });

const addTransaction = (transaction) => {
  transactions.value.push(transaction);
};

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter(t => t.id !== id);
};
</script>

<style scoped>
.app {
  background-color: #f8f9fa;
  padding: 1rem; /* Reduced padding */
  min-height: 100vh;
  display: flex;
  justify-content: center; /* Horizontally centers content */
  align-items: center; /* Vertically centers content */
}

.container {
  width: 100%;
  max-width: 800px; /* Ensures container doesn't exceed 800px */
  padding: 1rem; /* Reduced padding */
  background-color: white; /* Optional, to make the container stand out */
  border-radius: 8px; /* Optional, for rounded corners */
}

.app-title {
  text-align: center;
  font-size: 1.5rem; /* Reduced font size */
  margin-bottom: 1rem; /* Reduced margin */
}
</style>
