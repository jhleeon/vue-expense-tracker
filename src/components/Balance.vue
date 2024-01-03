<template>
    <h4>Youre Balance</h4>
    <h1 id="balance">${{ total }}</h1>
</template>

<script setup>
import { computed } from 'vue';

    const props = defineProps({
        total: {
            type:Number,
            required:true,
        }
    });

    // Format total to 2 decimal
    const total = computed(()=>{
        return props.total.toFixed(2); 
    });
</script>