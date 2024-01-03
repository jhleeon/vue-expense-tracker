<template>
    <div class="inc-exp-container">
        <div>
            <h4>Income</h4>
            <p id="money-plus" class="money plus">${{ income }}</p>
        </div>
        <div>
            <h4>Expense</h4>
            <p id="money-minus" class="money minus">${{ expense }}</p>
        </div>
    </div>
</template>

<script setup>
    import { computed } from 'vue';

    const props = defineProps({
        income: {
            type: Number,
            required: true
        },
        expense: {
            type: Number,
            required: true
        }
    });

    // Format income to 2 decimal
    const income = computed(()=>{
        return props.income.toFixed(2); 
    });

    // Format expense to 2 decimal
    const expense = computed(()=>{
        return props.expense.toFixed(2); 
    });
</script>