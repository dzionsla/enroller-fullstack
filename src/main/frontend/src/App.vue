<template>
  <div id="app">
    <h1>
      <img src="./assets/logo.svg" alt="Enroller" class="logo">
      System do zapisów na zajęcia {{ msg }}
    </h1>
    <div v-if="authenticatedUsername">
      <h2>Witaj {{ authenticatedUsername }}!
        <a @click="logout()" class="float-right  button-outline button">Wyloguj</a>
      </h2>
      <meetings-page :username="authenticatedUsername"></meetings-page>
    </div>
    <div v-else>
      <button @click='registering = false'> Zaloguj się </button>
      <button @click='registering = true'> Zarejestruj się </button>
      <p v-if="msg"> Logowanie się nie powiodło </p>	
      <login-form @login="login($event)"
      			  v-if="!registering"></login-form>
      <login-form @login="register($event)"
      			  button-label="Zarejestruj się"
      			  v-else="!registering"></login-form>
    </div>
  </div>
</template>

<script>
    import "milligram";
    import LoginForm from "./LoginForm";
    import MeetingsPage from "./meetings/MeetingsPage";

    export default {
        components: {LoginForm, MeetingsPage},
        data() {
            return {
                authenticatedUsername: "",
                registering: false,
                msg: false
                
            };
        },
        methods: {
            login(user) {
                this.authenticatedUsername = user.login;
            },
            logout() {
                this.authenticatedUsername = '';
            },
            register(user) {
            	 this.$http.post('participants', user)
            	     .then(response => {
            	         this.msg = false;// udało się
            	     })
            	     .catch(response => {
            	         this.msg = true;// nie udało sie     
            	     });
            }
        }
    };
</script>

<style>
  #app {
    max-width: 1000px;
    margin: 0 auto;
  }

  .logo {
    vertical-align: middle;
  }
</style>

