<template>
  <header class="container mt-5 w-75">
    <h3 class="text-center">Credit Card Management System</h3>
        
    <form class="w-50 mx-auto d-flex flex-column mt-5" @submit.prevent="onSubmit">
        <div class="alert alert-danger" role="alert" v-if="data.error">
          {{ data.error }}
        </div>
        <div class="mb-3">
          <label for="cardNumber" class="form-label">Card Number</label>
          <input type="text" class="form-control" id="cardNumber" minlength="16" placeholder="Enter your 16 digits card number" v-model="data.number">
        </div>

        <div class="mb-3">
          <label for="cardHolderName" class="form-label">Card Holder Name</label>
          <input type="text" class="form-control" id="cardHolderName" minlength="3" placeholder="Enter card holder's name" v-model="data.name">
        </div>

        <div class="mb-3">
          <label for="cvv" class="form-label">CVV</label>
          <input type="number" class="form-control" id="cvv" min="0" placeholder="Enter your 3 digits cvv" v-model="data.cvv">
        </div>

        <div class="mb-3">
          <label for="expiryDate" class="form-label">Expiry Date</label>
          <div id="expiryDate" class="d-flex gap-2">
            <input type="number" class="form-control" id="expiryMonth" min="0" max="12" aria-describedby="dateTip" placeholder="MM" v-model="data.month">
            <input type="number" class="form-control" id="expiryYear" min="0" aria-describedby="dateTip" placeholder="YY" v-model="data.year">
          </div>
        </div>
        
        <button type="submit" class="btn btn-primary btn-sm">Add</button>
    </form>

    <div class="w-75 mx-auto mt-5 mb-5" v-if="data.cards.length > 0">
      <h4 class="text-center">List Of Cards</h4>
      <ul class="list-group mt-3" v-for="(card, index) in data.cards" :key="index">
        <li class="list-group-item">
          <span class="fw-bold">{{card.name}}</span> holding account number 
          <span class="fw-light fst-italic">{{card.number}}</span>
          <button class="btn btn-danger btn-sm float-end" @click="deleteCard(index)">Delete</button>
        </li>
      </ul>
    </div>

  </header>
</template>


<script setup lang="ts">
import { reactive, watch } from 'vue';

let data: any = reactive({
  number: null,
  name: null,
  cvv: null,
  month: null,
  year: null,

  cards: [],
  error: null
})

const onSubmit = () => {
  if(data.number.length !== 16) {
    data.error = 'Card Number must be 16 digits number';
  } else if((data.cvv).toString().length !== 3) {
    data.error = 'CVV must be 3 digits number';
  } else if((data.year).toString().length !== 2) {
    data.error = 'Year must be in YY format';
  } else {
    let newCard = {
      number: data.number,
      name: data.name,
      cvv: data.cvv,
      month: data.month,
      year: data.year,
    };
    data.cards.push(newCard);
    data.number = data.name = data.cvv = data.month = data.year = null;
  }
}

const deleteCard = (index) => {
    data.cards.splice(index, 1);
}

</script>