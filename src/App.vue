<template>
  <div id="app" class="app">
    <div class="header">
      <img alt="Vue Header" src="./assets/Aplicacion-web-viajeros.png" />
      <nav>
        <button v-if="is_auth" v-on:click="loadPublications">
          Publicaciones
        </button>
        <button v-if="is_auth" v-on:click="loadAccount">Mi cuenta</button>
        <button v-if="is_auth" v-on:click="logOut">Cerrar Sesión</button>
        <button v-if="!is_auth" v-on:click="loadLogIn">Inicio de Sesión</button>
      </nav>
    </div>

    <div class="main-component">
      <router-view
        v-on:completedLogIn="completedLogIn"
        v-on:completedSignUp="completedSignUp"
        v-on:logOut="logOut"
      >
      </router-view>
    </div>

    <div class="footer">
      <h2>Misión TIC 2022</h2>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",

  data: function () {
    return {
      is_auth: false,
    };
  },

  components: {},

  methods: {
    verifyAuth: function () {
      this.is_auth = localStorage.getItem("isAuth") || false;

      if (this.is_auth == false) this.$router.push({ name: "logIn" });
      else this.$router.push({ name: "account" });
    },

    loadLogIn: function () {
      this.$router.push({ name: "logIn" });
    },

    completedLogIn: function (data) {
      console.log("Dentro de completedLogIn con " + data);
      localStorage.setItem("isAuth", true);
      localStorage.setItem("username", data.username);
      localStorage.setItem("token_access", data.token_access);
      localStorage.setItem("token_refresh", data.token_refresh);
      alert("Autenticación Exitosa");
      this.verifyAuth();
    },

    completedSignUp: function (data) {
      alert("Registro Exitoso");
      this.completedLogIn(data);
    },

    loadPublications: function () {
      this.$router.push({ name: "publications" });
    },

    loadAccount: function () {
      this.$router.push({ name: "account" });
    },

    logOut: function () {
      localStorage.clear();
      alert("Sesión Cerrada");
      this.verifyAuth();
    },
  },

  created: function () {
    this.verifyAuth();
  },
};
</script>

<style>
body {
  margin: 0;
  display: grid;
  min-height: 100vh;
  grid-template-rows: auto 1fr auto;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

.header {
  margin: 0%;
  padding: 0;
  width: 100%;

  background-color: #ffb800;
  color: #e5e7e9;

  position: relative;
}

.header img {
  max-height: 280px;
  width: 100%;
  text-align: center;
}

.header nav {

  margin: 20px;
  position: absolute;
  bottom: 0;
  right: 0;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.header nav button {
  color: #e5e7e9;
  background: #283747;
  border: 1px solid #e5e7e9;
  width: 110px;
  border-radius: 5px;
  padding: 10px;
  margin: 1px;
}

.header nav button:hover {
  color: #283747;
  background: #e5e7e9;
  border: 1px solid #e5e7e9;
}

.main-component {
  margin: 0%;
  padding: auto;

  background: #fdfefe;
}

.footer {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100px;

  background-color: #ffb800;
  color: #e5e7e9;
}

.footer h2 {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;

  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
