<template>
  <div class="signUp_user">
    <div class="container_slider">
      <ul class="slider">
        <li id="slide1">
          <div class="imagenes_falsas" style="background-color: red"></div>
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

    <div class="container_signUp_user">
      <h2>Registrarse</h2>

      <form v-on:submit.prevent="processSignUp">
        <input type="text" v-model="user.username" placeholder="Username" />
        <br />

        <input type="password" v-model="user.password" placeholder="Password" />
        <br />

        <input type="text" v-model="user.name" placeholder="Name" />
        <br />

        <input type="email" v-model="user.email" placeholder="Email" />
        <br />

        <input type="number" v-model="user.age" placeholder="Age" />
        <br />

        <input type="text" v-model="user.location" placeholder="Location" />

        <input
          type="text"
          v-model="user.description"
          placeholder="Your description (Optional)"
        />

        <button class="button" type="submit">Registrarse</button>
      </form>
    </div>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: "SignUp",

  data: function () {
    return {
      user: {
        username: "",
        password: "",
        name: "",
        email: "",
        age: "",
        location: "",
        description: "",
      },
    };
  },

  methods: {
    processSignUp: async function () {
      await this.$apollo
        .mutate({
          mutation: gql`
            mutation SignUpUser($userInput: SignUpInput) {
              signUpUser(userInput: $userInput) {
                refresh
                access
              }
            }
          `,
          variables: {
            userInput: this.user,
          },
        })
        .then((result) => {
          let dataSignUp = {
            username: this.user.username,
            token_access: result.data.access,
            token_refresh: result.data.refresh,
          };

          this.$emit("completedSignUp", dataSignUp);
        })
        .catch((error) => {
          console.log(error);
          alert("ERROR: Fallo en el registro.");
        });
    },
  },
};
</script>

<style>
.signUp_user {
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

.container_signUp_user {
  border: 3px solid #283747;
  border-radius: 10px;
  width: 25%;
  height: 390px;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.signUp_user h2 {
  visibility: hidden;
  color: #283747;
}

.signUp_user form {
  width: 70%;
}

.signUp_user input {
  height: 40px;
  width: 100%;

  box-sizing: border-box;
  padding: 10px 20px;
  margin: 5px 0;

  border: 1px solid #283747;
}

.signUp_user .button {
  max-width: 205px;
  width: 100%;
  height: 40px;

  color: #e5e7e9;
  background: #283747;
  border: 1px solid #e5e7e9;

  border-radius: 5px;
  padding: 10px 25px;
  margin: 5px 0 25px 0;
}

.signUp_user button:hover {
  color: #e5e7e9;
  background: crimson;
  border: 1px solid #283747;
}
</style>
