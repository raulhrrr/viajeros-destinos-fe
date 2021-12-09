<template>
  <h1>Hola</h1>
  <div class="container_general">
    <div class="container_general_signUp">
      <h2>Actualizar información de la cuenta</h2>

      <form>
        <input type="number" v-model="user.id" placeholder="id" readonly />
        <br />

        <input type="text" v-model="user.username" placeholder="username" />
        <br />

        <input type="text" v-model="user.name" placeholder="name" />
        <br />

        <input type="email" v-model="user.email" placeholder="email" />
        <br />

        <input type="number" v-model="user.age" placeholder="age" />
        <br />

        <input type="text" v-model="user.location" placeholder="location" />

        <input
          type="text"
          v-model="user.description"
          placeholder="description"
        />

        <button class="button" type="submit">Actualizar Información</button>
      </form>
    </div>
  </div>
</template>




<script>
import gql from "graphql-tag";
import jwt_decode from "jwt-decode";

export default {
  name: "ConfigUser",
  data: function () {
    return {
      userId: jwt_decode(localStorage.getItem("token_refresh")).user_id,
      userDetailById: {
        id: "",
        username: "",
        name: "",
        email: "",
        age: "",
        location: "",
        description: "",
      },
    };
  },

  apollo: {
    userDetailById: {
      query: gql`
        query UserDetailById($userId: Int!) {
          userDetailById(userId: $userId) {
            id
            username
            name
            email
            age
            location
            description
          }
        }
      `,
      variables() {
        return {
          userId: this.userId,
        };
      },
    },
  },

  methods: {
    processUpdateInfo: async function () {
      console.log("Dentro de processUpdateInfo");
      await this.$apollo
        .mutate({
          mutation: gql`
            mutation UpdateUser($userId: Int!, $userInput: UpdateUser!) {
              updateUser(userId: $userId, userInput: $userInput) {
                id
                username
                name
                email
                age
                location
                description
              }
            }
          `,
          variables: `
            {
              "userId": ${this.userDetailById.id},
              "userInput": {
                "username": "raulhrrr",
                "password": "admin",
                "name": "Raúl O. Herrera C.",
                "age": "18",
                "email": "raulherrera@gmail.com",
                "location": "Bogotá D.C",
                "description": "Intento de actualización de datos #2 para el usuario raulhrrr"
              }
            }
          `,
        })
        .then((result) => {
          console.log("Actualización de datos exitosa");
          console.log(result);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>






<style>
.container_general {
  margin: 0;
  padding: 0%;
  height: 100%;
  width: 100%;

  display: flex;
  justify-content: center;
  align-items: center;
}

.container_general_signUp {
  border: 3px solid #283747;
  border-radius: 10px;
  width: 25%;
  min-height: 400px;
  height: 60%;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.container_general h2 {
  color: #283747;
}

.container_general form {
  width: 70%;
}

.container_general input {
  height: 40px;
  width: 100%;

  box-sizing: border-box;
  padding: 10px 20px;
  margin: 5px 0;

  border: 1px solid #283747;
}

.container_general button {
  width: 100%;
  height: 40px;

  color: #e5e7e9;
  background: #283747;
  border: 1px solid #e5e7e9;

  border-radius: 5px;
  padding: 10px 25px;
  margin: 5px 0 25px 0;
}

.container_general button:hover {
  color: #e5e7e9;
  background: crimson;
  border: 1px solid #283747;
}
</style>
