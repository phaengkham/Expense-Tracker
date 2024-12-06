<script setup>
import { defineProps } from 'vue'

const props = defineProps({
    income: {
        type: [Number, String],
        requred: true,
    },
    expense: {
        type: [Number, String],
        requred: true,
    }
});


</script>

<template>
    <div class="inc-exp-container">
        <div>
            <h4>Incom</h4>
            <p class="money plus" id="money-plus">+${{ income }}</p>
        </div>
        <div>
            <h4>Expense</h4>
            <p id="money-minus" class="money minus">-${{ expense }}</p>
        </div>
    </div>
</template>