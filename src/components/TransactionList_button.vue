<template>
  <div>
    <h2>Transaction List</h2>

    <div>
      <!-- Radio buttons for filtering -->
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
          <td
            :class="{
              'text-success': transaction.type === 'Income',
              'text-danger': transaction.type === 'Expense',
              'fw-bold': transaction.amount >= 500
            }"
          >
            ${{ transaction.amount }}
          </td>
          <td class="text-uppercase">{{ transaction.type }}</td>
          <td>
            <button @click="deleteTransaction(transaction.id)" class="btn btn-danger btn-sm">Delete</button>
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
const filter = ref('all'); // Set the default filter to 'all'

const filteredTransactions = computed(() => {
  if (filter.value === 'all') return props.transactions;
  return props.transactions.filter(t => t.type.toLowerCase() === filter.value);
});
</script>

<style scoped>
.text-success { color: green; }
.text-danger { color: red; }

/* Optional styling for radio buttons */
label {
  margin-right: 1rem;
}
</style>
