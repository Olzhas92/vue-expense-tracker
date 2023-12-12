<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="number"
        id="amount"
        placeholder="Enter amount..."
        v-model="amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref(0);

const emit = defineEmits(["transactionSubmitted"]);

const toast = useToast();

const onSubmit = () => {
  if (text.value && amount.value) {
    toast.success("Transaction has been added");
    const newTransaction = {
      text: text.value,
      amount: parseFloat(amount.value),
    };

    emit("transactionSubmitted", newTransaction);
    text.value = "";
    amount.value = 0;
  } else {
    toast.error("Both fields must be filled");
    return;
  }
};
</script>

<style lang="css" scoped></style>
