 form="regisration_form"<template>
  <div id="app">
    <div class="form_wrapper">
      <form action="#" method="GET" id="registration_form" name="registration_form" @click="unlockSubmit">
        <header>
          <h3>Регистрация</h3>
          <div class="account_exists">
            <p>Уже есть аккаунт? </p>
          <a href="#">Войти</a>
        </div>
        </header>
        <label  class="name_label">
          <p class="label_header">Имя</p>
          <input type="text" placeholder="Введите ваше имя" autocomplete="off"  name="Имя" class="name" @input="nameValidate" required>
          <p class="error nameError hidden">{{errorMessage}}</p>
        </label>
        <label class="email_label">
          <p class="label_header">Email</p>
          <input type="email" placeholder="Введите ваш Email" autocomplete="off"  name="Email" class="email" @blur="emailValidate" required>
          <p class="error emailError hidden">{{errorMessage}}</p>
        </label>
        <label class="phone_label">
          <p class="label_header">Номер телефона</p>
          <input type="tel" placeholder="Введите ваш номер телефона" autocomplete="off"  name="phone" class="phone" @input="phoneValidate" @keydown="phoneClear">
          <p class="error phoneError hidden">{{errorMessage}}</p>
        </label>
        <label>
          <p class="label_header">Язык</p>
          <input class="hidden" type="text" autocomplete="off" :value="selectValue" name="language" required>
          <div class="select" @click="showOptions">
            {{selectValue}}
          </div>
          <div class="options hidden">
            <div class="option" @click="selectInput" v-for="language in languages" :key="language">{{ language}}</div>
          </div>
        </label>
        <label class="check">
          <input type="checkbox" class="hidden" name="agree" id="agree" @click="checkSwitch" required>
          <div class="checkbox"></div>
          <div class="description">Принимаю <a href="#">условия</a> использования</div>
        </label>
        <input type="submit" value="Зарегистрироваться" :disabled="disabled" class="submit_button">
        
      </form>

    </div>

  </div>
</template>

<script>


export default {
  data(){
    return {
      selectValue:"Язык",
      languages: ["Русский", "Английский", "Китайский", "Испанский"],
      disabled: true,
      nameValid: false,
      errorMessage: "Вы ввели неверные данные",
      nameIsValid: false,
      emailIsValid: false,
      phoneIsValid: false,
      numbersOnly: []
    }
  },
  methods: {
    selectInput(event){
      this.selectValue = event.target.textContent
      let options = document.querySelector('.options');
      options.classList.add("hidden");
      let selectLang = document.querySelector('.select');
      selectLang.classList.remove('active');
    },
    showOptions() {
      let options = document.querySelector('.options');
      options.classList.remove("hidden");
      let selectLang = document.querySelector(".select");
      selectLang.classList.add("active");
    },
    checkSwitch() {
      let checkbox = document.querySelector('.checkbox')
      checkbox.classList.toggle('checked');
    },
    unlockSubmit(){
      let submit = document.querySelector('.submit_button');
      if(this.nameIsValid && this.emailIsValid && this.phoneIsValid) {
        submit.disabled=false;
      }
    },
    nameValidate(){
      let nameError = document.querySelector('.nameError')
      let nameInput = document.querySelector('.name');
      let nameInputArr = nameInput.value.split('');
      nameError.classList.add('hidden');
      this.nameIsValid = true;
      for(let i=0;i<nameInputArr.length; i++) {
        if(nameInputArr[i].match(/[A-zА-я -]/i) == null) {
          nameInputArr.pop();
          let newNameInput = nameInputArr.join('');
          nameInput.value = newNameInput;
          nameError.classList.remove('hidden');
          this.nameIsValid = false;
          this.errorMessage = "Имя может состоять из букв, пробелов и знаков '-'"
        }
      }
      if(nameInput.value == "") {
        nameError.classList.remove('hidden');
        this.nameIsValid = false;

      }
    },
    emailValidate(){
      let emailError = document.querySelector('.emailError');
      let emailInput = document.querySelector('.email');
      emailError.classList.add('hidden');
      this.emailIsValid = true;
        if(emailInput.value.match(/([a-z0-9_-]+\.)*[a-z0-9_-]+@[a-z0-9_-]+(\.[a-z0-9_-]+)*\.[a-z]{2,6}$/i) == null) {
          emailError.classList.remove('hidden');
          this.emailIsValid = false;
          this.errorMessage = "Неправильный email, исправьте"
        }
      
    },
    phoneValidate(){
      let phoneError = document.querySelector('.phoneError')
      let phoneInput = document.querySelector('.phone');
      let phoneInputArr = phoneInput.value.split('');
      phoneError.classList.add('hidden');
      this.phoneIsValid = true;
      for(let i=0;i<phoneInputArr.length; i++) {
        if(phoneInputArr[i].match(/[0-9-)(+]/i) == null) {
          phoneInputArr.pop();
          let newPhoneInput = phoneInputArr.join('');
          phoneInput.value = newPhoneInput;
          phoneError.classList.remove('hidden');
          this.phoneIsValid = false;
        }
      }
      if(phoneInput.value == "") {
        phoneError.classList.remove('hidden');
        this.phoneIsValid = false;
      }
      for(let i=phoneInputArr.length-1;i<phoneInputArr.length; i++){
        if(phoneInputArr[i].match(/[0-9]/i) !== null) {
          this.numbersOnly.push(phoneInputArr[phoneInputArr.length-1]);
          phoneError.classList.add('hidden');
          if(this.numbersOnly.length<11) {
            phoneError.classList.remove('hidden');
            this.errorMessage = "Слишком короткий номер"
          }
          if(this.numbersOnly.length>11) {
            phoneError.classList.remove('hidden');
            this.errorMessage = "Слишком длинный номер"
          }
        }
      }
    },
    phoneClear(event){
      if(event.key == "Backspace") {
        let phoneInput = document.querySelector('.phone');
        phoneInput.value = '';
        this.numbersOnly = [];
      }
    }
  }
}
</script>

<style scoped>
  :active, :hover, :focus {
      outline: 0;
      outline-offset: 0;
  }
  #app {
    width: 100vw;
    height: 100vh;
    margin: 0;
    padding: 0;
    background: #ffffff;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .form_wrapper {
    position: relative;
    width: 460px;
    height: 100vh;
    margin: 0 auto;
    box-sizing: border-box;
    background-color: lightcoral;
    background: #FFFFFF;
    box-shadow: 0px 12px 24px rgba(44, 39, 56, 0.02), 0px 32px 64px rgba(44, 39, 56, 0.04);
    border-radius: 24px;
    overflow-y: scroll;

  }
  .form_wrapper::-webkit-scrollbar {
    width: 0px;
    background: #ffffff;
    right: 10px;
    position: absolute;
    background-clip: content-box;
    border-radius: 150px;
  }
  .form_wrapper::-webkit-scrollbar-thumb {
    height: 40px;
    width: 5px;
    background: #DBE2EA;
    border-radius: 15px;
  }

  #registration_form {
    width: 100%;
    min-height: 100vh;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: stretch;
    padding: 0 30px;
    box-sizing: border-box;
    background: #FFFFFF;
    box-shadow: 0px 12px 24px rgba(44, 39, 56, 0.02), 0px 32px 64px rgba(44, 39, 56, 0.04);
    border-radius: 24px;
  }

  header h3 {
    margin: 40px auto 0;
    font-family: 'IBM Plex Sans', sans-serif;
    font-style: normal;
    font-weight: bold;
    font-size: 34px;
    line-height: 44px;
    color: #2C2738;
  }
  .account_exists {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    font-family: 'IBM Plex Sans', sans-serif;
    font-style: normal;
    font-weight: normal;
    font-size: 16px;
    line-height: 22px;
  }
  .account_exists p {
    color: #2C2738;
    margin: 8px 0px;
  }
  .account_exists a {
    text-decoration: none;
    color: #0880AE;
    padding-left: 5px;
  }
  label {
    width: 100%;
    font-family: 'IBM Plex Sans', sans-serif;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items:stretch;
    position: relative;
  }
  label:first-of-type {
    margin-top: 50px;
  }
  .label_header {
    font-family: 'IBM Plex Sans', sans-serif;
    font-style: normal;
    font-weight: 500;
    font-size: 16px;
    line-height: 21px;
    color: #756F86;
    margin:0;
    margin-bottom: 7px;
  }
  label input, label .select {
    background: #FFFFFF;
    border: 1px solid #DBE2EA;
    box-sizing: border-box;
    box-shadow: 0px 4px 8px rgba(44, 39, 56, 0.04);
    border-radius: 6px;
    height: 52px;
    font-family: 'IBM Plex Sans', sans-serif;
    font-style: normal;
    font-weight: normal;
    font-size: 16px;
    line-height: 21px;
    color: #2C2738;
    padding: 16px;
    cursor: pointer;
  }

  label input::-webkit-input-placeholder{
    color: #7C9CBF;
  }
  label input::-moz-placeholder{
    color: #7C9CBF;
  }
  label input:-ms-input-placeholder {
    color: #7C9CBF;

  }
  label .select {
    position: relative;
  }
  label .select::after {
    content:"";
    width: 30px;
    height: 30px;
    background: url("./assets/images/Chevron Bottom.svg") center center no-repeat;
    position: absolute;
    right: 10px;
    top: 10px;
  }
  label .select.active {
    border: 2px solid #0880AE;
  }
  label .options {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    cursor: pointer;
    font-family: 'IBM Plex Sans', sans-serif;
    font-style: normal;
    font-weight: normal;
    border: 1px solid #DBE2EA;
    box-sizing: border-box;
  }
  label .options.hidden {
    display: none;
  }
  label .options .option {
    width: 100%;
    font-size: 16px;
    line-height: 44px;
    cursor: pointer;
    padding-left: 16px;
    box-sizing: border-box;
  }
  label .options .option:hover {
    background-color: #EBF4F8;
  }
  label input:focus {
    border: 2px solid #0880AE;
  }
  label input.hidden {
    display: none;
  }
    label p.error {
    visibility: visible;
    font-size: 14px;
    line-height: 18px;
    color: #FF7171;
    margin: 8px 0px;
  }
  label p.error.hidden {
    visibility: hidden;
  }

  label.check {
    width: 100%;
    height: 40px;
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
    margin: 37px auto;
  }
  label.check .description {
    margin:0;
  }

  label.check input[type="checkbox"] {
    display: none;
  }
  label.check .checkbox {
    margin-right: 10px;
    display: block;
    width: 30px;
    height: 30px;
    text-align: center;
    transform: translateY(4px);
    background: url('./assets/images/Base.svg') center/contain no-repeat;
    cursor: pointer;
  }
  label.check .checkbox.checked {
    background: url('./assets/images/Checked.svg') center/contain no-repeat;
  }
  label.check .description {
    font-family: 'IBM Plex Sans', sans-serif;
    font-style: normal;
    font-weight: 500;
    font-size: 16px;
    line-height: 21px;
    color: #756F86;
  }
  label.check .description a {
    color: #0880AE;
    white-space: pre;
    text-decoration: none;
    cursor: pointer;
  }
  input.submit_button {
    height: 56px;
    background: #0880AE;
    box-shadow: 0px 2px 4px rgba(44, 39, 56, 0.08), 0px 4px 8px rgba(44, 39, 56, 0.08);
    border-radius: 6px;
    border: none;
    font-family: 'IBM Plex Sans', sans-serif;
    font-style: normal;
    font-weight: 500;
    font-size: 16px;
    line-height: 21px;
    text-align: center;
    color: #EBF4F8;
    margin-bottom: 40px;
    cursor: pointer;
  }
  input.submit_button[disabled] {
    background-color: #DBE2EA;;
  }

  

  
  @font-face {
    font-family: 'IBM Plex Sans';
    src: local('IBM Plex Sans Text'), local('IBMPlexSans-Text'), url('./assets/fonts/Ibmplexsanstext.woff2') format('woff2'), url('./assets/fonts/Ibmplexsanstext.woff') format('woff'), url('./assets/fonts/Ibmplexsanstext.ttf') format('truetype');
    font-weight: 500;
    font-style: normal;
  }

  @font-face {
    font-family: 'IBM Plex Sans';
    src: local('IBM Plex Sans SemiBold'), local('IBMPlexSans-SemiBold'), url('./assets/fonts/Ibmplexsanssemibold.woff2') format('woff2'), url('./assets/fonts/Ibmplexsanssemibold.woff') format('woff'), url('./assets/fonts/Ibmplexsanssemibold.ttf') format('truetype');
    font-weight: 700;
    font-style: normal;
  }
  @font-face {
    font-family: 'IBM Plex Sans';
    src: local('IBM Plex Sans'), local('IBMPlexSans'), url('./assets/fonts/Ibmplexsans.woff2') format('woff2'), url('./assets/fonts/Ibmplexsans.woff') format('woff'), url('./assets/fonts/Ibmplexsans.ttf') format('truetype');
    font-weight: 400;
    font-style: normal;
  }

</style>
