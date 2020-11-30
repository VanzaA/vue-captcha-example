<template>
  <div id="container">
    <h1>Ejemplo usando vue-recaptcha</h1>
    <form id="form" @submit.prevent="onSubmit">
      <label for="user">Usuario</label>
      <input id="user" v-model="user" required />
      <label for="password">Contraseña</label>
      <input id="password" type="password" v-model="password" />
      <vue-recaptcha
        :sitekey="key"
        :loadRecaptchaScript="true"
        @verify="onVerify"
        @expired="onExpired"
        ref="recaptcha"
      />
      <div class="actions">
        <button class="btn btn-confirm">Iniciar</button>
        <button class="btn" type="button" @click="clean">
          Cancelar
        </button>
      </div>
      <span>Hint: admin admin</span>
    </form>
  </div>
</template>

<script>
import VueRecaptcha from "vue-recaptcha";
export default {
  name: "CaptchaForm",
  components: { VueRecaptcha },
  data: () => ({
    user: "",
    password: "",
    verified: false,
    key: process.env.VUE_APP_CAPTCHA_PUBLIC_KEY
  }),
  methods: {
    onSubmit() {
      if (this.user.length === 0 || this.password.length === 0) {
        alert("Usuario y contraseña son campos requeridos");
      }
      if (this.user === "admin" && this.password === "admin") {
        if (this.verified) {
          alert("Iniciaste correctamente");
        } else {
          alert("Es necesario validar captcha");
        }
      } else {
        alert("Datos invalidos");
        this.$refs.recaptcha.reset();
      }
    },
    clean() {
      this.user = "";
      this.password = "";
      this.verified = false;
      this.$refs.recaptcha.reset();
    },
    onVerify(response) {
      this.verified = !!response;
    },
    onExpired() {
      this.verified = false;
    }
  }
};
</script>

<style lang="scss" scoped>
#container {
  margin-top: 30px;
}
#form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  input {
    width: 400px;
    margin-bottom: 20px;
  }
  .actions {
    width: 400px;
    display: flex;
    justify-content: space-around;
    margin: 20px 0;
  }
  .btn {
    width: 180px;
    display: inline-block;
    margin-right: 2px;
    padding: 10px 25px;
    background: none;
    border: 1px solid #c0c0c0;
    border-radius: 2px;
    color: #666;
    font-size: 1.125em;
    outline: none;
    transition: all 100ms ease-out;
    &:hover,
    &:focus {
      transform: translateY(-3px);
    }
    &-confirm {
      border: 1px solid #2962ff;
      background: #2962ff;
      color: #fff;
    }
  }
}
span {
  color: red;
}
</style>
