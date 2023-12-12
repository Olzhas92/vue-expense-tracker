<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="item in transactions"
      :key="item.id"
      :class="item.amount > 0 ? 'plus' : 'minus'"
    >
      {{ item.text }} <span>${{ item.amount }}</span
      ><button class="delete-btn" @click="deleteTransaction(item.id)">x</button>
    </li>
  </ul>
</template>

<script setup>
const props = defineProps({
  transactions: { type: Array, required: true },
});

const emit = defineEmits(["deleteTransaction"]);

const deleteTransaction = (id) => {
  emit("deleteTransaction", id);
};
</script>

<style lang="css" scoped></style>
