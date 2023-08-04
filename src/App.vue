<script setup>
  import { ref } from "vue";

  const cvc = ref('')
  const cardNumber = ref('')
  const completeName = ref('')
  const month = ref('')
  const year = ref('')

  const saveCard = () => {
    console.log('oi');
  }

  const insertCvc = () => {
    cvc.value.length <= 1 ? cvc.value = cvc.value + '00' : console.log(cvc.value);
  }
</script>

<template>
  <header>
    <div class="header">
      <img class="header_mobile_img" src="./assets/images/bg-main-mobile.png" alt="background mobile">
      <img class="header_desktop_img" src="./assets/images/bg-main-desktop.png" alt="header background image" hidden>
      <div class="card">
        <div class="back_card">
          <img src="./assets/images/bg-card-back.png" alt="" class="back_card_img">
          <input type="text" class="cvc" v-model="cvc" placeholder="000" disabled>
        </div>
        <div class="front_card">
          <img src="./assets/images/bg-card-front.png" alt="front card image" class="front_card_img">
          <img src="./assets/images/card-logo.svg" alt="card logo" class="card_logo">
          <input type="text" class="complete_name" v-model="completeName" placeholder="Jane Appleseed" disabled>
          <input type="text" class="card_number" v-model="cardNumber" placeholder="0000 0000 0000 0000" disabled>
          <input type="text" class="month" v-model="month" placeholder="00" disabled>
          <span class="divisor">/</span>
          <input type="text" class="year" v-model="year" placeholder="00" disabled>
        </div>
      </div>
    </div>
  </header>
  <main>
    <form @submit.prevent="saveCard">
      <label>
        Cardholder Name
        <input type="text" v-model="completeName" placeholder="e.g. Jane Appleseed">
      </label>
      <label>
        Card Number
        <input type="text" v-model="cardNumber" placeholder="e.g. 1234 5678 9123 0000" maxlength="12">
      </label>
      <div class="exp_date">
        <label class="month_year">
          Exp date (MM/YY)
          <div class="month_year_input">
            <input class="input_month" type="text" v-model="month" placeholder="MM" maxlength="2">
            <input class="input_year" type="text" v-model="year" placeholder="YY" maxlength="2">
          </div>
        </label>
        <label class="label_cvc">
          Cvc
          <input class="input_cvc" type="text" v-model="cvc" placeholder="e.g. 123" maxlength="3">
        </label>
      </div>
      <button>Confirm</button>
    </form>
  </main>

  <section hidden>
      Thank you!
      We've added your card details
      Continue
  </section>

  <footer class="attribution">
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>. 
    Coded by <a href="#">Your Name Here</a>.
  </footer>
</template>

<style scoped>
  .header{
    position: relative;
  }
  .header_mobile_img {
    min-height: 15em;
    max-height: 15em;
  }
  .card{
    margin-top: -13em;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .back_card, .front_card {
    position: relative;
    width: 100%;
    max-width:  18em;
    margin-right: -4em;
    max-height: 14em;
  }
  .card_logo {
    position: absolute;
    width: 4em;
    left: 1em;
    top: 1em;
  }
  .cvc, .complete_name, .card_number, .month, .year, .divisor {
    position: absolute;
    color: var(--white);
    letter-spacing: 0.2em;
    border: none;
    background-color: transparent;
    font-size: 10px;
  }
  .cvc {
    top: 5.5em;
    left: 22em;
  }
  .card_number{
    left: 0.2em;
    top: 3.7em;
    font-size: 17px;
    letter-spacing: 0.2em;
  }
  .complete_name, .month, .year, .divisor {
    text-transform: uppercase;
    top: 11em;
    left: 1em;
  }
  .month {
    left: 21.8em;
  }
  .divisor{
    left: 24.3em;
    top: 12.5em;
  }
  .year{
    left: 23.8em
  }

  .cvc::placeholder, .complete_name::placeholder, .month::placeholder, .year::placeholder{
    color: var(--white);
    font-size: 10px;
    letter-spacing: 0.1em;
  }
  .card_number::placeholder{
    font-size: 18px;
    letter-spacing: 0.1em;
  }
  .front_card {
    margin: -4.55em 4em 0 0;
  }
  main{
    display: flex;
    justify-content: center;
  }
  form{
    width: 90%;
    margin-top: 2em;
    max-width: 30em;
  }
  label {
    text-transform: uppercase;
    letter-spacing: 0.15em;
    font-size: 12px;
  }
  input {
    margin: 0.6em 0 1.5em;
    padding: 1em;
    border: none;
    border: 1px solid var(--light-grayish-violet);
    border-radius: 8px;
  }
  input::placeholder {
    color: var(--light-grayish-violet);
    font-size: 18px;
  }
  .exp_date {
    display: flex;
  }
  .month_year_input{
    display: flex;
    padding-right: 1.5em;
    gap: 1em;
    width: 100%;
  }
  .label_cvc {
    width: 110%;
  }
  button {
    border: none;
    background-color: var(--very-dark-violet);
    color: var(--white);
    font-size: 18px;
    padding: 0.8em 0;
    border-radius: 8px;
    margin-bottom: 3em;
  }
  footer {
    position: absolute;
    bottom: 1em;
  }
  .attribution { font-size: 11px; text-align: center; }
  .attribution a { color: hsl(228, 45%, 44%); }
</style>
