<template>
<div>

  <form v-if="state ==='register'">
    <p>
      Создайте аккаунт
    </p>
    <div class="input">
      <input type="text" v-model="reg.login">
      <label :class='reg.login.length > 0 ? "placeholder-up" : "placeholder"'>Имя пользователя</label>
      <span v-if="isLoginCorrect" class="correct">✔</span>
    </div>
    <div class="input">
      <input type="e-mail" v-model="reg.email">
      <label :class='reg.email.length > 0 ? "placeholder-up" : "placeholder"'>Адрес эл. почты</label>
      <span v-if="isEmailCorrect" class="correct">✔</span>
    </div>
    <div class="input">
      <input class="input-password" :type='reg.showPassword ? "text" : "password"' v-model="reg.password">
      <label :class='reg.password.length > 0 ? "placeholder-up" : "placeholder"'>Пароль</label>
      <span :class='reg.showPassword ? "eye" : "crossed eye"' @click="switchShowPass('reg', 'showPassword')"></span>
    </div>
    <div class="input">
      <input class="input-password" :type='reg.showConfirm ? "text" : "password"' v-model="reg.confirm">
      <label :class='reg.confirm.length > 0 ? "placeholder-up" : "placeholder"'>Подтвердить</label>
      <span :class='reg.showConfirm ? "eye" : "crossed eye"' @click="switchShowPass('reg', 'showConfirm')"></span>
    </div>
    <input type="button" value="Зарегистрироваться" :disabled="!isValidReg">
    <span class="small-button" @click="switchState('login')">Войти</span>
  </form>
  <form v-if="state === 'login'">
    <p>
      Войти в аккаунт
    </p>
    <div class="input">
      <input type="text" v-model="log.login">
      <label :class='log.login.length > 0 ? "placeholder-up" : "placeholder"'>Имя пользователя</label>
    </div>
    <div class="input">
      <input class="input-password" :type='log.showPassword ? "text" : "password"' v-model="log.password">
      <label :class='log.password.length > 0 ? "placeholder-up" : "placeholder"'>Пароль</label>
      <span :class='log.showPassword ? "eye" : "crossed eye"' @click="switchShowPass('log', 'showPassword')"></span>
    </div>
    <input type="button" value="Войти" :disabled="!isValidLog">
    <span class="small-button" @click="switchState('register')">Зарегистрироваться</span>
  </form>
</div>
</template>

<script>
export default {
  name: 'Main',
  data () {
    return {
      state: 'register',
      reg: {
        login: '',
        email: '',
        password: '',
        showPassword: false,
        confirm: '',
        showConfirm: false,
        error: ''
      },
      log: {
        login: '',
        password: '',
        showPassword: false,
        error: ''
      }

    }
  },
  computed: {
    isLoginCorrect () {
      return this.reg.login.length > 1
    },
    isEmailCorrect () {
      // eslint-disable-next-line no-useless-escape
      const re = /^(([^<>()\[\]\.,;:\s@\"]+(\.[^<>()\[\]\.,;:\s@\"]+)*)|(\".+\"))@(([^<>()[\]\.,;:\s@\"]+\.)+[^<>()[\]\.,;:\s@\"]{2,})$/i
      return re.test(this.reg.email)
    },
    isPasswordOk () {
      if (this.reg.password !== this.reg.confirm) return false
      return this.reg.password.length >= 5
    },
    isValidReg () {
      return this.isLoginCorrect && this.isEmailCorrect && this.isPasswordOk
    },
    isValidLog () {
      return this.log.login.length > 1 && this.log.password.length > 1
    }
  },
  methods: {
    switchShowPass (form, input) {
      this[form][input] = !this[form][input]
    },
    switchState (state) {
      this.state = state
    }
  }
}
</script>

<style scoped>
  * {
    font-size: 1rem;
    margin: 0;
    box-sizing: border-box;
  }
  form {
    padding: 2rem;
    margin: 2rem auto;
    max-width: 500px;
    display: grid;
    grid-gap: 1rem;
    border: solid 1px #ccc;
    border-radius: 0.5rem;
    grid-template-columns: max-content 1fr max-content;
  }
  p {
    font-size: 1.6rem;
  }
  .input {
    position: relative;
    grid-column: 1 / -1;
    display: flex;
    flex-direction: column;
  }
  .input-password {
    margin-right: 3rem;
  }
  .input input:focus~.placeholder {
    transform: translate(0, -1.6rem) scale(0.8);
  }
  .input input:focus~label {
    color: #4285F4;
  }
  .placeholder {
    position: absolute;
    top: 0.6rem;
    left: 0.6rem;
    pointer-events: none;
    background-color: #fff;
    padding: 0.2rem;
    transition: 0.1s;
    transform-origin: left;
    color: #555;
  }
  .placeholder-up {
    position: absolute;
    top: 0.6rem;
    left: 0.6rem;
    pointer-events: none;
    background-color: #fff;
    padding: 0.2rem;
    transform-origin: left;
    transform: translate(0, -1.6rem) scale(0.8);
  }

  .small-button {
    grid-column: 1 / 3;
    border: 0;
    color: #4285F4;
    width: max-content;
    margin: auto;
    font-size: 0.9rem;
    cursor: pointer;
  }
  .small-button:hover {
    text-decoration: underline;
  }
  .eye {
    cursor: pointer;
    position: absolute;
    right: 0;
    width: 2.8rem;
    height: 2.8rem;
    background-image: url("../assets/eye.svg");
    background-position: center;
    background-repeat: no-repeat;
    background-size: 1.6rem;
  }
  .eye:hover {
    color: #555;
  }

  .crossed {
    background-image: url("../assets/eye_cross.svg");
  }
  input[type="text"],
  input[type="password"],
  input[type="e-mail"] {
    padding: 0.8rem;
    border-radius: 0.2rem;
    box-shadow: 0 0 0 1px #ccc;
    border: 0;
  }
  input[type="text"]:focus,
  input[type="password"]:focus,
  input[type="e-mail"]:focus {
    outline: 0;
    box-shadow: 0 0 0 2px #4285F4;
  }
  input[type="button"] {
    grid-column: 1 / 3;
    width: max-content;
    margin: auto;
    padding: 0.2rem 1rem;
    border-radius: 0.2rem;
    border-width: 1px;
  }
  input[type="button"]:enabled {
    background-color: #4285F4;
    color: #fff;
    cursor: pointer;
  }
  input[type="button"]:hover:enabled {
    background-color: #0a52c7;
  }

  .correct {
    color: #008000;
    position: absolute;
    right: 1rem;
    top: 1rem;
    pointer-events: none;
  }
</style>
