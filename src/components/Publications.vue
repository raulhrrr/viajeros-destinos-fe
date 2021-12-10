<template>
  <div class="contenido_publications">
    <ul>
      <li v-for="publicacion in publicaciones" v-bind:key="publicacion.id">
        <div class="publicacion">
          <h2>Publicación #{{ publicacion.id }}</h2>
          <span><b>Localización del lugar:</b> {{ publicacion.location }}</span>
          <span
            ><b>Foto de la ubicación:</b>
            <a href=""> {{ publicacion.image }} </a></span
          >
          <span><b>Id propietario del post:</b> {{ publicacion.id_user }}</span>
          <span><b>Fecha de la publicación:</b> {{ publicacion.date }}</span>
          <span> <b> Descripción: </b> {{ publicacion.description }}</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: "Publications",
  data: function () {
    return {
      publicaciones: [],
    };
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
.contenido_publications {
  width: 100%;
  height: 100%;
}

.publicacion {
  margin: 30px auto;
  padding: 20px;
  border: 1px solid black;
  width: 70%;
  height: 200px;
  overflow: auto;
}

.publicacion span {
  display: block;
}
</style>