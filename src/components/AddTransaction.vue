<template>
    <h3>Add New Transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" v-model="text" placeholder="Enter text...">
        </div>
        <div class="form-control">
            <label for="amount">
                Amount <br/> (negtive - expense, postive - income)
            </label>
            <input type="text" id="amount" v-model="amount" placeholder="Enter amount...">
        </div>
        <button class="btn">Add Transaction</button>
    </form>
</template>

<script setup>
    import { ref } from 'vue';
    import { useToast } from "vue-toastification";

    const toast = useToast();

    const emit = defineEmits(['transactionSubmited']);

    const text = ref('');
    const amount = ref('');

    // Transaction add form submit event
    const onSubmit  = () => {
        if(!text.value || !amount.value) {
            toast.error("Both filed must be filled.");
        }
        
        const transactionData = {
            text: text.value,
            amount: parseFloat(amount.value),
        };

        emit('transactionSubmited', transactionData);

        text.value = '';
        amount.value = '';
    }
</script>