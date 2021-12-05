<template>
  <!-- <div v-if="loaded" class="portada"> -->
  <div class="portada">
    <div class="color">
      <img src="#" alt="" />
    </div>
  </div>
  <div class="contenido_account">
    <div class="lado_izquierdo">
      <div class="descripcion"></div>
      <div class="publicaciones"></div>
    </div>
    <div class="lado_derecho"></div>
  </div>
</template>

<script>
// import jwt_decode from "jwt-decode";
// import axios from "axios";

// export default {
//   name: "Account",
//   data: function () {
//     return {
//       username: "",
//         password: "",
//         name: "",
//         email: "",
//         age: "",
//         location: "",
//         description: "",
//     };
//   },

//   methods: {
//     getData: async function () {
//       if (
//         localStorage.getItem("token_access") === null ||
//         localStorage.getItem("token_refresh") === null
//       ) {
//         this.$emit("logOut");
//         return;
//       }
//       await this.verifyToken();
//       let token = localStorage.getItem("token_access");
//       let userId = jwt_decode(token).user_id.toString();
//       axios
//         .get(
//           `https://mision-tic-bank-
//     be.herokuapp.com/user/${userId}/`,
//           { headers: { Authorization: `Bearer ${token}` } }
//         )
//         .then((result) => {
//           this.name = result.data.name;
//           this.email = result.data.email;
//           this.balance = result.data.account.balance;
//           this.loaded = true;
//         })
//         .catch(() => {
//           this.$emit("logOut");
//         });
//     },
//     verifyToken: function () {
//       return axios
//         .post(
//           "https://mision-tic-bank-be.herokuapp.com/refresh/",
//           { refresh: localStorage.getItem("token_refresh") },
//           { headers: {} }
//         )
//         .then((result) => {
//           localStorage.setItem("token_access", result.data.access);
//         })
//         .catch(() => {
//           this.$emit("logOut");
//         });
//     },
//   },
//   created: async function () {
//     this.getData();
//   },
// };
//
</script>

<style>
.portada {
  margin: 20px auto;
  padding: 0;
  width: 90%;
  height: 150px;
  border: 1px solid black;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.portada .color {
  margin: 20px;
  height: 95%;
  width: 95%;
  background-color: red;
  border: 1px solid black;
}

.portada h1 {
  font-size: 60px;
  color: #0f1316;
}
.portada h2 {
  font-size: 40px;
  color: #283747;
}
.portada span {
  color: crimson;
  font-weight: bold;
}

.contenido_account {
  margin: 20px auto;
  padding: 0;
  width: 90%;
  height: 500px;
  border: 1px solid black;

  display: flex;
  justify-self: space-between;
  align-items: center;
}

.lado_izquierdo {
  height: 100%;
  width: 70%;
  border: 1px solid black;
}

.lado_izquierdo .descripcion {
  margin: auto;
  height: 50%;
  width: 95%;
  border: 1px solid black;
}

.lado_izquierdo .publicaciones {
  margin: auto;
  height: 50%;
  width: 95%;
  border: 1px solid black;
}

.lado_derecho {
  height: 100%;
  width: 30%;
  border: 1px solid black;
}
</style>
