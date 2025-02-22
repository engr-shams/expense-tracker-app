<template>
  <div class="transaction-list">
    <!-- <h2 class="list-title">Transaction List</h2> -->

    <div class="filter-options">
      <label>
        <input type="radio" name="filter" value="all" v-model="filter" /> All
      </label>
      <label>
        <input type="radio" name="filter" value="income" v-model="filter" /> Income
      </label>
      <label>
        <input type="radio" name="filter" value="expense" v-model="filter" /> Expense
      </label>
    </div>

    <table class="table mt-3">
      <thead>
        <tr>
          <th>Title</th>
          <th>Amount</th>
          <th>Type</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="transaction in filteredTransactions" :key="transaction.id">
          <td>{{ transaction.title }}</td>
          <td :class="{
                'text-success': transaction.type === 'Income',
                'text-danger': transaction.type === 'Expense',
                'fw-bold': transaction.amount >= 500
              }">
            ${{ transaction.amount }}
          </td>
          <td class="text-uppercase">{{ transaction.type }}</td>
          <td>
            <button @click="deleteTransaction(transaction.id)" class="btn btn-delete">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>

    <p v-if="transactions.length === 0">No transactions recorded yet.</p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps(['transactions', 'deleteTransaction']);
const filter = ref('all');

const filteredTransactions = computed(() => {
  if (filter.value === 'all') return props.transactions;
  return props.transactions.filter(t => t.type.toLowerCase() === filter.value);
});
</script>

<style scoped>
.transaction-list {
  background-color: #fff;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.list-title {
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.filter-options {
  margin-bottom: 1.5rem;
}

.filter-options label {
  margin-right: 1rem;
}

.table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 0.8rem;
  text-align: left;
}

th {
  background-color: #f1f1f1;
}

.text-success { color: green; }
.text-danger { color: red; }

.btn-delete {
  background-color: #e74c3c;
  color: white;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.btn-delete:hover {
  background-color: #c0392b;
}
</style>
