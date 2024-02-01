<template>
    <Header />
    <div class="container">
        <Balance :total="total" />
        <IncomeExpenses :income="+income" :expenses="+expenses" />
        <TransactionList :transactions="transactions" @delete-transaction="handleDeletedTransaction" />
        <AddTransaction @add-transaction="handleTransactionSubmitted" />
    </div>
</template>

<script setup>
import Balance from "./components/Balance.vue";
import Header from "./components/Header.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";
import "vue-toastification/dist/index.css";

const toast = useToast();

const transactions = ref([]);

onMounted (() => {
    getTransactions();
});

// Get transactions from local storage
const getTransactions = () => {
    transactions.value = JSON.parse(localStorage.getItem("transactions")) || [];
};

// Get total
const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

// Get income
const income = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2);
});

// Get expenses
const expenses = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2);
});

// add transaction
const handleTransactionSubmitted = (transaction) => {
    transactions.value.push({
        id: transaction.id,
        text: transaction.text,
        amount: transaction.amount,
    });
    saveTransactions();
};

// delete transaction
const handleDeletedTransaction = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
    toast.success("Transaction Deleted");
    saveTransactions();

};

// Save transactions to local storage
const saveTransactions = () => {
    localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
