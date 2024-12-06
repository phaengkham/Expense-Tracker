<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    total: {
        type: Number, 
        requred: true
    }
})
</script>

<template>
    <div>
        <h4>Your Balance</h4>
        <h2 id="balance">${{ total }}</h2>
    </div>
</template>