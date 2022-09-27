<template>
  <div class="container main-wrapper">
    <div class="main_">
      <h2 class="text-center text-white">Payment Methods</h2>
      <div class="form-group">
        <label class="form-labels labels_text" for="card_number"
          >Card Number</label
        >
        <input
          class="form-control input-login"
          id="card_number"
          name="card_number"
          placeholder="Enter your card number"
          v-model="data.newData.cardNumber"
          maxlength="19"
          pattern="[0-9]*"
          inputmode="numeric"
        />
      </div>
      <div class="form-group">
        <label class="form-labels labels_text" for="name">Full Name</label>
        <input
          type="text"
          class="form-control input-login"
          id="name"
          name="name"
          placeholder="Enter your full name"
          v-model="data.newData.name"
        />
      </div>
      <div class="row d-flex justify-content-between">
        <div class="col-md-5">
          <div class="form-group">
            <label class="form-labels labels_text" for="exp_date"
              >Expiry Date</label
            >
            <input
              type="text"
              class="form-control input-login"
              id="exp_date"
              name="exp_date"
              v-model="data.newData.date"
              placeholder="MM/YY"
              maxlength="5"

            />
          </div>
        </div>
        <div class="col-md-5">
          <div class="form-group">
            <label class="form-labels labels_text" for="email">CCV</label>
            <input
            type="number"
              class="form-control input-login"
              id="ccv"
              name="ccv"
              v-model="data.newData.ccv"
              placeholder="3 digit CCV"
              maxlength="3"
              inputmode="numeric"
            />
          </div>
        </div>
      </div>
      <button
        @click="addPaymentMethod()"
        class="btn btn-primary add_payment_method"
      >
        Add Payment Method
      </button>
      <div class="mt-3">
        <h4 class="text-center text-white">Payment method lists</h4>
        <div
          v-if="data.paymentMethods.length>0"
          v-for="(paymentMethod, index) in data.paymentMethods"
          class="row list"
        >
          <div class="col-md-8 info">
            <p class="c_number">{{paymentMethod.cardNumber}}</p>
            <p class="name">{{paymentMethod.name}}</p>
          </div>
          <div class="col-md-4 actions">
            <!-- <button @click="active(index)" v-if="!paymentMethod.is_active" class="btn btn-success active">Active</button> -->
            <button @click="remove(index)" class="btn btn-danger">Remove</button>
          </div>
        </div>
        <div v-else class="nothing">No payment method added</div>
      </div>
    </div>
  </div>
</template>

<!-- Script -->
<script setup lang="ts">
import { reactive, watch } from "vue";
interface formData {
    cardNumber: String,
    name: String,
    date: String,
    ccv: String,
    is_active: Boolean,
}
interface dataTypes {
  paymentMethods: any[];
  newData: formData;
}
let data = reactive<dataTypes>({
  paymentMethods: [],
  newData: {
    cardNumber: "",
    name: "",
    date: "",
    ccv: "",
    is_active: false,
  },
});
const addPaymentMethod = () => {
  let date  = new Date();
  let month = date.getMonth();
  let year =String(date.getFullYear()).slice(-2);
  if (
    data.newData.cardNumber === "" ||
    data.newData.name === "" ||
    data.newData.date === "" ||
    data.newData.ccv === ""
  ) {
    alert("Please fill add fields");
    return;
  }
  if(data.newData.cardNumber.length!==19){
    alert('Please enter correct card number')
    return
  }
  if(Number(data.newData.date.split('/')[0])<month || Number(data.newData.date.split('/')[1])<Number(year)){
    alert('Please enter valid date')
    return
  }
  let newData = data.newData;

  data.paymentMethods.push(newData);
};
const remove = (index:number)=>{
  data.paymentMethods.splice(index,1);
}
const active = (index:number)=>{
  for(let i=0;i<data.paymentMethods.length;i++){
    data.paymentMethods[i].is_active = false;
  }
  data.paymentMethods[index].is_active = true
}
watch(data, (newVal: any) => {
  // Card number without dash (-)
  if (newVal.newData.cardNumber !== "") {
    let realNumber = String(newVal.newData.cardNumber).replace(/-/gi, "");

    // Generate dashed number
    let dashedNumber: any = realNumber.match(/.{1,4}/g);

    // Replace the dashed number with the real one
    data.newData.cardNumber = dashedNumber.join("-");
  }
  if (newVal.newData.name !== "") {
    data.newData.name = newVal.newData.name;
  }
  if (newVal.newData.date !== "") {
    
    data.newData.date = newVal.newData.date.replace(/^(\d\d)(\d)$/g,'$1/$2').replace(/^(\d\d\/\d\d)(\d+)$/g,'$1/$2').replace(/[^\d\/]/g,'');

  }
  if (newVal.newData.ccv !== "") {
    data.newData.ccv = newVal.newData.ccv;
  }
});
</script>

<!-- Styles -->
<style scoped>
.main-wrapper {
  padding: 50px;
  width: 50%;
}
.labels_text {
  color: #ffffff;
  padding: 10px 0 3px 0;
  font-size: 14px;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none !important;
  margin: 0 !important;
}
.add_payment_method {
  width: 100%;
  margin-top: 20px;
  background-color: #7caa7c;
  border-color: #7caa7c;
  height: 55px;
  font-weight: bold;
}
.list {
  align-items: center;
  margin: 10px 0;
  background: #5e9a5e;
  padding: 10px;
  border-radius: 5px;
  box-shadow: 2px 2px 2px 1px rgb(0 0 0 / 20%);
}
.info {
  color: #ffffff;
  padding: 0;
}
.actions {
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}
.active_ {
  margin-right: 5px;
}
p {
  margin: 0;
}
.nothing{
  color:#fff;
  text-align: center;
}
</style>
