<script setup>
  import { ref } from "vue";

  const cvc = ref('')
  const cvcInvalid = ref(false)
  const cardNumber = ref('')
  const cardNumberInvalid = ref(false)
  const completeName = ref('')
  const completeNameInvalid = ref(false)
  const month = ref('')
  const monthInvalid = ref(false)
  const year = ref('')
  const yearInvalid = ref(false)
  const cardSaved = ref(false)

  const formatCvc = e => cvc.value  = e.target.value.replace(/\D/g, '')
  const formatMonth = e => month.value  = e.target.value.replace(/\D/g, '')
  const formatYear = e => year.value  = e.target.value.replace(/\D/g, '')
  const formatCardNumber = e => {
    const cardLength = cardNumber.value.length
    cardNumber.value  = e.target.value.replace(/\D/g, '')
    let bl1 = cardNumber.value.substring(0,4)
    let bl2 = cardNumber.value.substring(4,8)
    let bl3 = cardNumber.value.substring(8,12)
    let bl4 = cardNumber.value.substring(12,16)
    bl1.length == 4 ? bl1 = bl1 + ' ' : null
    bl2.length == 4 ? bl2 = bl2 + ' ' : null
    bl3.length == 4 ? bl3 = bl3 + ' ' : null
    cardNumber.value = bl1 + bl2 + bl3 + bl4
    cardLength <= 5 && e.inputType == 'deleteContentBackward' ? cardNumber.value = cardNumber.value.substring(0,4) : null
    cardLength > 5 && cardLength <= 10 && e.inputType == 'deleteContentBackward' ? cardNumber.value = bl1 + cardNumber.value.substring(5,9) : null
    cardLength > 10 && cardLength <= 15 && e.inputType == 'deleteContentBackward' ? cardNumber.value = bl1 + bl2 + cardNumber.value.substring(10,14) : null
  }
  const saveCard = () => {
    if (cvc.value.length!==3) return
    if (month.value.length!==2) return
    cardSaved.value = !cardSaved.value
  }
  const backToCardForm = () => {
    clearForm()
    cardSaved.value = !cardSaved.value
  } 
  const clearForm = () => {
    cvc.value = ''
    cardNumber.value = ''
    completeName.value = ''
    month.value = ''
    year.value = ''
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
          <input type="text" class="cvc" :value="cvc.padEnd(3,'0')" placeholder="000" disabled>
        </div>
        <div class="front_card">
          <img src="./assets/images/bg-card-front.png" alt="front card image" class="front_card_img">
          <img src="./assets/images/card-logo.svg" alt="card logo" class="card_logo">
          <input type="text" class="complete_name" v-model="completeName" placeholder="Jane Appleseed" disabled>
          <input type="text" class="card_number" v-model="cardNumber" placeholder="0000 0000 0000 0000" disabled @input="formatCardNumber">
          <input type="text" class="month" v-model="month" placeholder="00" disabled>
          <span class="divisor">/</span>
          <input type="text" class="year" v-model="year" placeholder="00" disabled>
        </div>
      </div>
    </div>
  </header>
  <main :class="{card_saved: cardSaved}">
    <form @submit.prevent="saveCard">
      <label>
        Cardholder Name
        <input type="text" v-model="completeName" placeholder="e.g. Jane Appleseed">
      </label>
      <label>
        Card Number
        <input type="text" v-model="cardNumber" placeholder="e.g. 1234 5678 9123 0000" maxlength="19" pattern="^\d{4}\s\d{4}\s\d{4}\s\d{4}$" @input="formatCardNumber" >
      </label>
      <div class="exp_date">
        <label class="month_year">
          Exp date (MM/YY)
          <div class="month_year_input">
            <input class="input_month" type="text" v-model="month" placeholder="MM" maxlength="2" @input="formatMonth">
            <input class="input_year" type="text" v-model="year" placeholder="YY" maxlength="2" @input="formatYear">
          </div>
        </label>
        <label class="label_cvc">
          Cvc
          <input class="input_cvc" type="text" v-model="cvc" placeholder="e.g. 123" maxlength="3" @input="formatCvc">
        </label>
      </div>
      <button type="submit">Confirm</button>
    </form>
  </main>

  <section class="continue" :hidden=!cardSaved>
    <img class="complete_icon" src="./assets/images/icon-complete.svg" alt="complete icon">
      <h1>Thank you!</h1>
      <p>We've added your card details</p>
      <button type="button" @click="backToCardForm">Continue</button>
  </section>

  <footer class="attribution">
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>. 
    Coded by <a href="#">Your Name Here</a>.
  </footer>
</template>

<style scoped>
  .header_mobile_img {
    height: 15em;
    width: 100%;
  }
  .card{
    margin-top: -13em;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .back_card, .front_card {
    position: relative;
    max-width:  18em;
  }
  .back_card_img {
    position: absolute;
    left: 1.8em;
  }
  .front_card_img{
    position: absolute;
    top: 5.6em;
    right: 1.8em;
  }
  .card_logo {
    position: absolute;
    width: 4em;
    left: -0.6em;
    top: 7em;
  }
    input {
    margin: 0.6em 0 1.5em;
    padding: 1em;
    border: none;
    border: 1px solid var(--light-grayish-violet);
    border-radius: 8px;
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
    width: 5em;
    left: 25em;
  } 
  .card_number{
    left: -1.5em;
    top: 9em;
    font-size: 17px;
    letter-spacing: 0.2em;
  }
  .complete_name, .month, .year, .divisor {
    text-transform: uppercase;
    top: 20em;
    left: 1em;
  }
  .complete_name{
    width: 20em;
    left: -1.8em;
  }
  .month {
    left: 19em;
    width: 5em;
  }
  .divisor{
    left: 21.5em;
    top: 21.5em;
    width: 1em;
  }
  .year{
    width: 5em;
    left: 21em
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
  main{
    display: flex;
    justify-content: center;
    margin-top: 16.5em;
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
  input::placeholder {
    color: var(--light-grayish-violet);
    font-size: 18px;
  }
  .exp_date {
    display: flex;
  }
  .month_year_input{
    display: flex;
    padding-right: 1em;
    gap: 0.6em;
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
  section {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .complete_icon {
    margin-top: 18em;
    width: 4em;
  }
  h1, p {
    width: fit-content;
  }
  h1 {
    text-transform: uppercase;
    margin-top: 1em;
    margin-bottom: 0.8em;
    letter-spacing: 0.2em;
    color: var(--very-dark-violet);
  }
  p {
    color: var( --dark-grayish-violet);
    font-size: 17px;
  }
  section button {
    width: 90%;
    max-width: 25em;
    margin-top: 2.5em;
  }
  .card_saved {
    display: none;
  }
  .attribution { font-size: 11px; text-align: center; }
  .attribution a { color: hsl(228, 45%, 44%); }
</style>
