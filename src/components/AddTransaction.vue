<template>
  <div class="add-transaction">
    <form @submit.prevent="handleSubmit">
      <div class="input-group">
        <div class="input-item">
          <input type="text" v-model="title" class="form-control" placeholder="Title" required />
        </div>

        <div class="input-item">
          <input type="number" v-model="amount" class="form-control" placeholder="Amount ($)" required min="1" />
        </div>

        <div class="input-item">
          <select v-model="type" class="form-control" required>
            <option value="" selected>Type</option>
            <option value="Income">Income</option>
            <option value="Expense">Expense</option>
          </select>
        </div>

        <button type="submit" class="btn btn-add">Add</button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const emit = defineEmits(['addTransaction']);
const title = ref('');
const amount = ref('');
const type = ref('');

const handleSubmit = () => {
  if (!title.value || amount.value <= 0 || !type.value) {
    alert('Please enter valid details.');
    return;
  }

  const newTransaction = {
    id: Date.now(),
    title: title.value,
    amount: Number(amount.value),
    type: type.value,
  };

  emit('addTransaction', newTransaction);
  title.value = '';
  amount.value = '';
  type.value = '';
};
</script>

<style scoped>
.add-transaction {
  background-color: #fff;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.input-group {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.input-item {
  margin-right: 1rem;
}

input, select {
  width: 200px;
}

button.btn-add {
  background-color: #3498db;
  color: white;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button.btn-add:hover {
  background-color: #2980b9;
}
</style>
