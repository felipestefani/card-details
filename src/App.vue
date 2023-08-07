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
  const checkChange = el => {
    const id = el.target.id
    if(el.key === 'Enter') return
    id === 'complete_name' ? completeNameInvalid.value = false : 
    id === 'card_number' && cardNumber.value.length == 19 ? document.getElementById('month').focus() : 
    id === 'card_number' ? cardNumberInvalid.value = false : 
    id === 'month' && month.value.length == 2 ? document.getElementById('year').focus() : 
    id === 'month' ? monthInvalid.value = false : 
    id === 'year' ? yearInvalid.value = false : 
    id === 'cvc' ? cvcInvalid.value = false : ''
  }
  const isValidName = () => completeName.value.length < 2 ? completeNameInvalid.value = true : ''
  const isValidCardNumber = () => cardNumber.value.length !== 19 ? cardNumberInvalid.value = true : ''
  const isMonthInvalid = () => month.value.length < 2 ? monthInvalid.value = true : ''
  const isYearInvalid = () => year.value.length < 2 ? yearInvalid.value = true : ''
  const isCvcInvalid = () => cvc.value.length < 3 ? cvcInvalid.value = true : ''
  const saveCard = () => {
    isValidName()
    isValidCardNumber()
    isMonthInvalid()
    isYearInvalid()
    isCvcInvalid()
    if(completeNameInvalid.value || cardNumberInvalid.value || monthInvalid.value || yearInvalid.value || cvcInvalid.value) return
    cardSaved.value = true
  }
  const backToCardForm = () => {
    clearForm()
    cardSaved.value = false
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
  <div class="container">
  <header>
    <div class="header">
      <img class="header_mobile_img" src="./assets/images/bg-main-mobile.png" alt="background mobile">
      <img class="header_desktop_img" src="./assets/images/bg-main-desktop.png" alt="header background image">
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
        <input id="complete_name" :class="{input_error: completeNameInvalid}" type="text" v-model="completeName" placeholder="e.g. Jane Appleseed" @keyup="checkChange">
        <span v-if="completeNameInvalid" class="error">Isert a valid name</span>
      </label>
      <label>
        Card Number
        <input id="card_number" type="text" :class="{input_error: cardNumberInvalid}" v-model="cardNumber" placeholder="e.g. 1234 5678 9123 0000" maxlength="19" @input="formatCardNumber" @keyup="checkChange" >
        <span class="error" v-if="cardNumberInvalid">Isert the correct card number</span>
      </label>
      <div class="exp_date">
        <label class="month_year">
          Exp date (MM/YY)
          <div class="month_year_input">
            <input id="month" :class="{input_error: monthInvalid}" class="input_month" type="text" v-model="month" placeholder="MM" maxlength="2" @input="formatMonth" @keyup="checkChange">
            <input id="year" :class="{input_error: yearInvalid}" class="input_year" type="text" v-model="year" placeholder="YY" maxlength="2" @input="formatYear" @keyup="checkChange">
          </div>
          <span class="error" v-if="monthInvalid || yearInvalid">All fields required</span>
        </label>
        <label class="label_cvc">
          Cvc
          <input id="cvc" :class="{input_error: cvcInvalid}" class="input_cvc" type="text" v-model="cvc" placeholder="e.g. 123" maxlength="3" @input="formatCvc" @keyup="checkChange">
          <span class="error" v-if="cvcInvalid">Isert the correct cvc</span>
        </label>
      </div>
      <button type="submit">Confirm</button>
    </form>
  </main>

  <section class="continue" :class="{card_saved: !cardSaved}">
    <div class="continue_container">
    <img class="complete_icon" src="./assets/images/icon-complete.svg" alt="complete icon">
      <h1>Thank you!</h1>
      <p>We've added your card details</p>
      <button type="button" @click="backToCardForm">Continue</button>
    </div>
  </section>

  <footer class="attribution">
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>. 
    Coded by <a href="#">Your Name Here</a>.
  </footer>
</div>
</template>

<style scoped>
  .header_mobile_img {
    height: 15em;
    width: 100%;
  }
  .header_desktop_img {
    display: none;
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
    width: 16em;
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
    display: flex;
    flex-direction: column;
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
    align-items: center;
    justify-content: center;
  }
  .continue_container {
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
  .error{
    font-size: 11px;
    color: var(--red);
    margin: -1em 0 1.5em;
    text-transform:none;
    letter-spacing: 0em;
  }
  .input_error {
    border-color: var(--red) !important;
  }
  .card_saved {
    display: none;
  }
  .attribution { font-size: 11px; text-align: center; }
  .attribution a { color: hsl(228, 45%, 44%); }

  input:hover {
    cursor: pointer;
  }

  input:focus {
    outline: none;
    background: linear-gradient(white, white) padding-box,
                linear-gradient(to right,hsl(249, 99%, 64%), hsl(278, 94%, 30%)) border-box;
    border-radius: 8px;
    border: 1px solid transparent;
  }

  @media (min-width: 1100px) {
    .container{
      display: flex;
      min-height: 100vh;
    }
    .header{
      width: 60%;
      min-height: 100vh;
    }
    .header_desktop_img {
      display: block;
      height: 100vh;
    }
    .header_mobile_img {
      display: none;
    }
    .card {
      margin-top: -30em;
    }
    .front_card, .back_card {
      max-width: none;
      width:25em;
    }
    .front_card {
      top: -18em;
      left: 10em;
    }
    .back_card{
      top: 3.5em;
      left: 11em;
    }
    .card_number::placeholder {
      font-size: 25px;
    }
    .cvc::placeholder, .complete_name::placeholder, .month::placeholder, .year::placeholder {
      font-size: 14px;
    }
    .cvc, .complete_name, .month, .year{
      font-size: 14px;
    }
    .cvc {
      top: 5.5em;
      left: 24em;
    }
    .card_number{
      top: 11em;
      width: 22em;
      left: -1em;
    }
    .complete_name, .month, .year {
      top: 17em;
      left: -1em;
    }
    .month{
      left: 20em;
    }
    .divisor{
      top: 26.2em;
      left: 31.6em;
    }
    .year{
      left: 22em;
    }
    main {
      align-self: center;
      margin-top: 0em;
      justify-content: flex-start;
      width: 100%;
      padding-left: 2em;
    }
    form {
      width: 90%;
    }
    label {
      width: 90%;
      font-size: 14px;
    }
    .exp_date{
      width: 90%;
    }
    .month_year {
      padding-right: 0.5em;
    }
    .label_cvc {
      width: 100%;
    }
    button {
      margin-top: 1em;
      width: 90%;
    }
    section{
      justify-content: flex-start;
    }
    .continue_container {
      width: fit-content;
      padding-left: 4em;
    }
    .complete_icon {
      margin-top: 0em;
    }
    section button {
      min-width: 22em;
    }
  }

  @media (max-width: 1099px) and (min-width: 840px) {
    .container{
      display: flex;
      min-height: 100vh;
    }
    .header{
      width: 60%;
      min-height: 100vh;
    }
    .header_desktop_img {
      display: block;
      min-height: 100vh;
    }
    .header_mobile_img {
      display: none;
    }
    .card {
      margin-top: -30em;
    }
    .front_card, .back_card {
      max-width: none;
      width:20em;
    }
    .front_card {
      top: -16em;
      left: 8em;
    }
    .back_card{
      top: 2em;
      left: 8em;
    }
    .card_number::placeholder {
      font-size: 20px;
    }
    .cvc::placeholder, .complete_name::placeholder, .month::placeholder, .year::placeholder {
      font-size: 12px;
    }
    .cvc, .complete_name, .month, .year{
      font-size: 12px;
    }
    .cvc {
      top: 5em;
      left: 22.5em;
    }
    .card_number{
      top: 9.5em;
      width: 22em;
      left: -1.2em;
    }
    .complete_name, .month, .year {
      top: 17.3em;
      left: -1.2em;
    }
    .month{
      left: 18.3em;
    }
    .divisor{
      top: 22.8em;
      left: 25em;
    }
    .year{
      left: 20.3em;
    }
    main {
      align-self: center;
      margin-top: 0em;
      justify-content: flex-start;
      width: 100%;
      padding-left: 4em;
    }
    form {
      width: 90%;
    }
    label {
      width: 90%;
      font-size: 12px;
    }
    input::placeholder {
      font-size: 14px;
    }
    .exp_date{
      width: 90%;
    }
    .month_year {
      padding-right: 0.5em;
    }
    .label_cvc {
      width: 100%;
    }
    button {
      margin-top: 1em;
      width: 90%;
    }
    .complete_icon{
      margin-top: 0em;
    }
    section button {
      width: 80%;
    }
  }
</style>
