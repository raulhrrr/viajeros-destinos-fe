<template>
  <div class="logIn_user">
    <div class="container_slider">
      <ul class="slider">
        <li id="slide1">
          <img src="../assets/slide-viajeros.png" alt="">
        </li>
        <li id="slide2">
          <div class="imagenes_falsas" style="background-color: black"></div>
        </li>
        <li id="slide3">
          <div class="imagenes_falsas" style="background-color: blue"></div>
        </li>
        <li id="slide4">
          <div class="imagenes_falsas" style="background-color: green"></div>
        </li>
      </ul>

      <ul class="menu">
        <li>
          <a href="#slide1">1</a>
        </li>
        <li>
          <a href="#slide2">2</a>
        </li>
        <li>
          <a href="#slide3">3</a>
        </li>
        <li>
          <a href="#slide4">4</a>
        </li>
      </ul>
    </div>

    <div class="container_logIn_user">
      <h2>Iniciar sesión</h2>

      <form v-on:submit.prevent="processLogInUser">
        <input type="text" v-model="user.username" placeholder="Username" />
        <br />
        <input type="password" v-model="user.password" placeholder="Password" />
        <br />
        <button class="button" type="submit">Iniciar Sesion</button>
      </form>

      <h3>No tienes una cuenta?</h3>
      <button class="button-go" v-on:click="loadSignUp">Registrarse</button>
    </div>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: "LogIn",

  data: function () {
    return {
      user: {
        username: "",
        password: "",
      },
    };
  },

  methods: {
    processLogInUser: async function () {
      await this.$apollo
        .mutate({
          mutation: gql`
            mutation LogIn($credentials: CredentialsInput!) {
              logIn(credentials: $credentials) {
                refresh
                access
              }
            }
          `,
          variables: {
            credentials: this.user,
          },
        })
        .then((result) => {
          let dataLogIn = {
            username: this.user.username,
            token_access: result.data.logIn.access,
            token_refresh: result.data.logIn.refresh,
          };
          this.$emit("completedLogIn", dataLogIn);
        })
        .catch((error) => {
          alert("ERROR " + error);
        });
    },

    loadSignUp: function () {
      this.$router.push({ name: "signUp" });
    },
  },
};
</script>

<style>
.logIn_user {
  margin: 0;
  padding: 0 100px;
  height: calc(100vh - 150px);
  width: calc(100% - 200px);

  display: flex;
  justify-content: space-between;
  align-items: center;
}

.container_slider {
  background-color: white;
  height: 350px;
  width: 60%;
}

ul,
li {
  padding: 0;
  margin: 0;
  list-style: none;
}

ul.slider {
  position: relative;
  height: inherit;
  width: 100%;
}

ul.slider li {
  position: absolute;
  left: 0px;
  top: 0px;
  opacity: 0;
  width: inherit;
  height: inherit;
  transition: opacity 0.5s;
  background: #fff;
}

ul.slider li .imagenes_falsas {
  width: 100%;
  height: 100%;
  border: 1px solid black;
  border: 2px solid black;
  object-fit: cover;
}

ul.slider li:first-child {
  opacity: 1; /*Mostramos el primer <li>*/
}

ul.slider li:target {
  opacity: 1; /*Mostramos el <li> del enlace que pulsemos*/
}

.menu {
  text-align: center;
  margin: 10px;
}

.menu li {
  display: inline-block;
  text-align: center;
  padding: 0 2px;
}

.menu li a {
  display: inline-block;
  color: white;
  text-decoration: none;
  background-color: grey;
  padding: 10px;
  width: 15px;
  height: 15px;
  font-size: 15px;
  border-radius: 100%;
}

.container_logIn_user {
  border: 3px solid #283747;
  border-radius: 10px;
  width: 25%;
  height: 390px;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.logIn_user h2 {
  display: none;
  color: #283747;
}

.logIn_user form {
  width: 70%;
}

.logIn_user input {
  height: 40px;
  width: 100%;

  box-sizing: border-box;
  padding: 10px 20px;
  margin: 5px 0;

  border: 1px solid #283747;
}

.logIn_user .button {
  max-width: 205px;
  width: 100%;
  height: 40px;

  color: #e5e7e9;
  background: #283747;
  border: 1px solid #e5e7e9;

  border-radius: 5px;
  padding: 10px 25px;
  margin: 5px 0;
}

.logIn_user .button-go {
  max-width: 205px;
  width: 100%;
  height: 40px;

  border: 1px solid;

  border-radius: 5px;
  padding: 10px 25px;
  margin: 5px 0;
}

.logIn_user button:hover {
  color: #e5e7e9;
  background: crimson;
  border: 1px solid #283747;
}
</style>
