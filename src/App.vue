<template>
    <Header />
    <div class="container">
        <Balance :total="total" />
        <IncomeExpenses :income="income" :expense="expense" />
        <TransactionList :transactions='transactions' @transaction-deleted="handleTransactionDeleted" />
        <AddTransaction @transaction-submited="handleTransactionSubmited" />
    </div>
</template>

<script setup>
    import Header from './components/Header.vue';
    import Balance from './components/Balance.vue';
    import IncomeExpenses from './components/IncomeExpenses.vue';
    import TransactionList from './components/TransactionList.vue';
    import AddTransaction from './components/AddTransaction.vue';
    import { ref, computed, onMounted } from 'vue';
    import { useToast } from "vue-toastification";

    const toast = useToast();

    const transactions = ref([]);

    onMounted(() => {
        const saveTransactions = JSON.parse(localStorage.getItem('transactions'));
        if (saveTransactions) {
            transactions.value = saveTransactions;
        }
    })

    // Get total
    const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0);
    });

    // Get income
    const income = computed(() => {
    return transactions.value
        .filter((transaction) => { return transaction.amount > 0})
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0);
    });

    // Get expense
    const expense = computed(() => {
    return transactions.value
        .filter((transaction) => { return transaction.amount < 0})
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0);
    });

    // Add transacton
    const handleTransactionSubmited = (transactionData) => {
        transactions.value.push({
            id: generateUniqueId(),
            text: transactionData.text,
            amount: transactionData.amount,
        });
        saveTransactionToLocalStorage();
        toast.success('Transaction added');
    };

    // Generate unique ID
    const generateUniqueId = () => {
        return Math.floor(Math.random()*1000000);
    };

    // Delete transaction
    const handleTransactionDeleted = (id) => {
        transactions.value = transactions.value.filter((transaction) => {
        return  transaction.id !== id;
        });
        saveTransactionToLocalStorage();
        toast.success('Transaction removed');
    };

    // Save to localstorage
    const saveTransactionToLocalStorage = () => {
        localStorage.setItem('transactions', JSON.stringify(transactions.value));
    };

</script>