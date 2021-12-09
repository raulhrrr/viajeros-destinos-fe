<template>
  <!-- <div v-if="loaded" class="portada"> -->
  <div class="portada">
    <div class="color">
      <img src="#" alt="" />
    </div>
  </div>
  <div class="contenido_account">
    <div class="lado_izquierdo">
        <h3>Tus datos:</h3>
      <div class="descripcion">
        <span><b>ID: </b>{{ userDetailById.id }}</span>
        <span><b>Username: </b>{{ userDetailById.username }}</span>
        <span><b>Nombre: </b>{{ userDetailById.name }}</span>
        <span><b>Email: </b>{{ userDetailById.email }}</span>
        <span><b>Edad: </b>{{ userDetailById.age }}</span>
        <span><b>Localización: </b>{{ userDetailById.location }}</span>
        <span><b>Descripción: </b>{{ userDetailById.description }}</span>
      </div>
    </div>
    <div class="lado_derecho">
      <h3>Publicaciones realizadas por ti:</h3>
      <ul>
      <li v-for="publicacion in publicaciones" v-bind:key="publicacion.id">
        <div class="publicacion" v-if="publicacion.id_user == userDetailById.id">
          <h2>Publicación #{{ publicacion.id }}</h2>
          <span><b>Localización del lugar:</b> {{ publicacion.location }}</span>
          <span
            ><b>Foto de la ubicación:</b>
            <a href="">{{ publicacion.image }}</a></span
          >
          <span><b>Id propietario del post:</b> {{ publicacion.id_user }}</span>
          <span><b>Fecha de la publicación:</b> {{ publicacion.date }}</span>
          <span> <b> Descripción: </b> {{ publicacion.description }}</span>
        </div>
      </li>
    </ul>
    </div>
  </div>
</template>

<script>
import gql from "graphql-tag";
import jwt_decode from "jwt-decode";

export default {
  name: "Account",
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
      publicaciones: [],
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

  async mounted() {
    try {
      var vue = this;
      var resultado = await this.$apollo.query({
        query: gql`
          query GetAllPublications {
            getAllPublications {
              id
              location
              description
              image
              id_user
              date
            }
          }
        `,
      });
      vue.publicaciones = resultado.data.getAllPublications;
      console.log(vue.publicaciones);
    } catch (error) {
      console.log(error);
    }
  },
};
</script>

<style>
.descripcion span {
  display: block;
  padding: 1rem;
  font-size: 1.5rem;
}

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

  display: flex;
  justify-self: space-between;
  align-items: center;
}

.contenido_account h3 {
  margin: 20px;
}

.lado_izquierdo {
  height: 100%;
  width: 60%;
  border: 1px solid black;
  overflow: auto;
}

.lado_izquierdo .descripcion {
  padding: 20px;
  margin: auto;
  height: 100%;
  width: 90%;
  border: 1px solid black;
  overflow: auto;
}

.lado_derecho {
  height: 100%;
  width: 40%;
  border: 1px solid black;
  overflow: auto;
}
</style>
