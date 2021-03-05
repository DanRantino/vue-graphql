<template>
  <div id="app">
    <div id="nav">
    Ola
    <ul>
      <li v-for="livro of Livros" :key="livro.identificador">
      <div>{{livro.identidicador}}</div> <div>{{livro.autor}}</div> <div>{{livro.titulo}}</div>
      </li>
    </ul>-
    </div>
    <router-view/>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data()
  {
    return {
      Livros: [],
      id:1
    }
  },
 async mounted() {
    var result = await axios({
      method: "POST",
      params: this.id,
      url: "http://localhost:5000/",
      data: { 
        query: `
        {
          getLivro(identificador: ${this.id}) 
          {
            identificador,
            titulo,
            autor
          }
        }`
      }
    });
    console.log(result.data.data.getLivro)
    this.Livros = result.data.data.getLivro
  },
}
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
