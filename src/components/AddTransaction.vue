<template>
    <h3>Add new transaction</h3>
      <form id="form" @submit.prevent = "onSubmit">
        <div class="form-control">
          <label for="text">Text</label>
          <input type="text" id="text" v-model="text" placeholder="Enter text..." />
        </div>
        <div class="form-control">
          <label for="amount"
            >Amount <br />
            (negative - expense, positive - income)</label
          >
          <input type="number" id="amount" v-model="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
      </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast} from "vue-toastification";

const text = ref("");
const amount = ref("");
const toast = useToast();
const emit = defineEmits(["add-transaction"]);

const onSubmit = () => {
    if (!text.value || !amount.value){
        toast.error("Please fill in the values");
        return;
    }

    const transactionData = {
        id: Math.floor(Math.random() * 100000000),
        text: text.value,
        amount: parseFloat(amount.value),
    };
    
    emit("add-transaction", transactionData);
    toast.success("Transaction added successfully");

    text.value = "";
    amount.value = "";
}
</script>