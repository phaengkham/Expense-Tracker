<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";


const text = ref('');
const amount = ref('');
const toast = useToast('');

const emit = defineEmits(['transactionSubmitted'])
const onSubmit = () => {
   if (!text.value || !amount.value) {
      toast.error('Both fields must be filled');

   };
   const transactionData = {
      text: text.value,
      amount: parseFloat(amount.value)
   };
   emit('transactionSubmitted', transactionData)
   text.value = '';
   amount.value = '';
};
</script>

<template>
   <h3>Add new transaction</h3>
   <form id="form" @submit.prevent="onSubmit">
      <div class="form-control">
         <label for="text">text</label>
         <input type="text" id="text" v-model="text" placeholder="Enter text...">
      </div>
      <div>
         <label for="amount">Amount <br>(negative - expense, psitive - incom)</label>
         <input type="text" id="amount" v-model="amount" placeholder="Enter amount...">
      </div>
      <button class="btn">Add Transaction</button>
   </form>
</template>