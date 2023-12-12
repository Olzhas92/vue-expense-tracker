<template>
  <div>
    <Header />
    <div class="container">
      <Balance :total="total" />
      <IncomeExpenses :income="+income" :expense="+expense" />
      <TransactionList
        :transactions="transactions"
        @deleteTransaction="deleteTransaction"
      />
      <AddTransaction @transactionSubmitted="transactionSubmitted" />
    </div>
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

const transactions = ref([
  { id: 1, text: "Flower", amount: -20 },
  { id: 2, text: "Salary", amount: 300 },
  { id: 3, text: "Book", amount: -10 },
  { id: 4, text: "Camera", amount: 150 },
]);

const total = computed(() => {
  return transactions.value.reduce((acc, item) => {
    return acc + item.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((item) => item.amount > 0)
    .reduce((acc, item) => {
      return acc + item.amount;
    }, 0)
    .toFixed(2);
});

const expense = computed(() => {
  return transactions.value
    .filter((item) => item.amount < 0)
    .reduce((acc, item) => {
      return acc + item.amount;
    }, 0)
    .toFixed(2);
});

const transactionSubmitted = (newTransaction) => {
  const transactionData = {
    id: generateUniqueID(),
    text: newTransaction.text,
    amount: newTransaction.amount,
  };

  transactions.value.push(transactionData);
  toast.success("Transaction has been added");
};

function generateUniqueID() {
  return Math.floor(Math.random() * 1000000);
}

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter((item) => item.id !== id);

  toast.success("Transaction has been deleted");
};
</script>

<style lang="css" scoped></style>
