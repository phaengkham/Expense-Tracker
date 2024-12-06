<script setup>

import Header from '@/components/Header.vue'
import Balance from '@/components/Balance.vue'
import IncomeExpenses from '@/components/IncomeExpenses.vue'
import TransactionList from '@/components/TransactionList.vue'
import AddTransaction from '@/components/AddTransaction.vue'
import { ref, computed, onMounted } from 'vue'
import { useToast } from 'vue-toastification'

const transactions = ref([]);

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
    if (savedTransactions) {
        transactions.value = savedTransactions
    }
});

const saveTransactionToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
}

const total = computed(() =>
    transactions.value
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2)
);

const income = computed(() =>
    transactions.value
        .filter(transaction => transaction.amount > 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2)
);


const expense = computed(() =>
    transactions.value
        .filter(transaction => transaction.amount < 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2)
);

const toast = useToast();

const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount
    });
    saveTransactionToLocalStorage();
    toast.success('Transaction added')
};

const generateUniqueId = () => {
    Math.floor(Math.random() * 1000000)
}

const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
    toast.success('Transaction Deleted');
    saveTransactionToLocalStorage();
};
</script>

<template>
    <Header></Header>
    <div class="container">
        <Balance :total="+total"></Balance>
        <IncomeExpenses :income="+income" :expense="+expense"></IncomeExpenses>
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"></TransactionList>
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted"></AddTransaction>
    </div>
</template>