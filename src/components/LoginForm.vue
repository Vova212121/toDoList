<template>
  <form class="login-form" @submit.prevent="login">
    <h1 class="login-form__title">Log In</h1>
    <div class="login-form__section">
      <label
        for="username"
        class="login-form__section__label"
        :class="{ active: userInputFocus || username }"
        >Username</label
      >
      <input
        class="login-form__section__input"
        type="text"
        id="username"
        @focus="userInputFocus = true"
        @focusout="userInputFocus = false"
        v-model="username"
        @input="validateUsername"
      />
      <div v-show="!usernameValid" class="login-form__section__errors">
        Please enter a valid alphabetic username
      </div>
    </div>
    <div class="login-form__section">
      <label
        for="phone"
        class="login-form__section__label"
        :class="{ active: phoneInputFocus || phone }"
        >Phone</label
      >
      <input
        class="login-form__section__input"
        type="tel"
        id="phone"
        @focus="phoneInputFocus = true"
        @focusout="phoneInputFocus = false"
        v-model="phone"
        @input="validatePhone"
      />
      <div v-show="!phoneValid" class="login-form__section__errors">
        Please enter a valid phone number
      </div>
    </div>
    <button class="login-form__button" type="submit" :disabled="!formValid">
      Login
    </button>
    <div v-if="error" class="login-form__error">
      Login errors / username or phone
    </div>
  </form>
</template>

<script>
export default {
  name: "LoginForm",
  data() {
    return {
      username: "",
      phone: "",
      error: false,
      usernameValid: true,
      phoneValid: true,
      users: [],
      userInputFocus: false,
      phoneInputFocus: false,
    };
  },
  mounted() {
    this.getUsers();
  },
  computed: {
    formValid() {
      return (
        this.usernameValid &&
        this.phoneValid &&
        this.username !== "" &&
        this.phone !== ""
      );
    },
  },
  watch: {
    error() {
      if (this.error) {
        setTimeout(() => {
          this.error = false;
        }, 3000);
      }
    },
  },
  methods: {
    validateUsername() {
      this.username = this.username.replace(/[^a-zA-Z]/g, "");
      this.usernameValid = /^[a-zA-Z]+$/.test(this.username);
    },
    validatePhone() {
      this.phone = this.phone.replace(/[^0-9x\-(). ]/g, "");
      this.phoneValid = /^[0-9x\-(). ]+$/.test(this.phone);
    },
    getUsers() {
      const url = `https://jsonplaceholder.typicode.com/users`;
      fetch(url)
        .then((response) => response.json())
        .then((data) => {
          console.log("DATA", data);
          this.users = data;
        });
    },
    login() {
      for (const user of this.users) {
        if (user.username === this.username && user.phone === this.phone) {
          this.successfulLogin(user.id);
          return;
        }
      }
      this.error = true;
    },
    successfulLogin(id) {
      this.$router.push({
        name: "user",
        params: { id: id },
      });
    },
  },
};
</script>

<style lang="scss">
.login-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 420px;
  background-color: #fafafb;
  border: 1px solid #121116;
  padding: 40px;
  position: relative;
  border-radius: 10px;
  .login-form__title {
    font-size: 30px;
    font-weight: 700;
    margin-bottom: 40px;
    text-transform: uppercase;
  }
  .login-form__section {
    position: relative;
    width: 100%;
    .login-form__section__label {
      font-size: 12px;
      color: #121116;
      transition: all 0.2s linear;
      position: absolute;
      top: 15px;
      left: 12px;
      &.active {
        transition: all 0.2s linear;
        font-size: 10px;
        top: 5px;
      }
    }
    .login-form__section__input {
      width: 100%;
      box-sizing: border-box;
      padding: 10px 12px 0 12px;
      height: 45px;
      border-radius: 5px;
      margin-bottom: 20px;
      font-weight: 700;
      border: 1px solid #121116;
      &:focus {
        outline: none !important;
        border: 1px solid #656565;
      }
    }
    .login-form__section__errors {
      position: absolute;
      bottom: 3px;
      font-size: 12px;
      color: #121116;
      @extend .blink;
    }
  }
  .login-form__button {
    width: 100%;
    height: 45px;
    border-radius: 5px;
    text-transform: uppercase;
    font-size: 16px;
    font-weight: 700;
    color: white;
    background-color: #121116;
    cursor: pointer;
    transition: all 0.1s linear;
    border: 1px solid transparent;
    &:hover {
      background-color: white;
      border: 1px solid #121116;
      color: #121116;
    }
    &:disabled {
      border: 1px solid #cececf;
      background-color: transparent;
      color: #cececf;
    }
  }
  .login-form__error {
    position: absolute;
    bottom: 10px;
    font-size: 14px;
    transition: all 0.2s linear;
    @extend .blink;
  }
}

.blink {
  animation-name: blinker;
  animation-iteration-count: infinite;
  animation-timing-function: cubic-bezier(1, 0, 0, 1);
  animation-duration: 2s;
  -webkit-animation-name: blinker;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-timing-function: cubic-bezier(1, 0, 0, 1);
  -webkit-animation-duration: 2s;
}

@keyframes blinker {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
@-webkit-keyframes blinker {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
</style>
