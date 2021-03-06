<template>
  <div id="app">
    <table v-if="this.id == ''" style="width:90%;text-align:center">
    <tr v-for="livro in Livros" :key="livro.identificador">
      <th>{{livro.titulo}}</th>
      <th>{{livro.autor}}</th>
    </tr>
    </table>
    <table v-else style="width:90%;text-align:center">
    <tr>
      <th>{{Livro.titulo}}</th>
      <th>{{Livro.autor}}</th>
    </tr>
    </table>
    <input class="color" type="text" name="Digite o identificador do livro" placeholder="Digite o identificador do livro" @change="getLivro" v-model="id" id="CodLivro"> 
    <h1>{{Livroencontrado}}</h1>
  </div>
</template>
<script>
import axios from 'axios'

export default {
  data()
  {
    return {
      Livros: [],
      Livro:'',
      id:'',
      Livroencontrado: ''
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
          getLivros 
          {
            identificador,
            titulo,
            autor
          }
        }`
      }
    });
    this.Livros = result.data.data.getLivros
  },
  methods: {
    async getLivro() 
    {
      if (this.id == 0){
        this.id = ''
      }
      if (this.id != ''){
        var result = await axios({
        method: "POST",
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
      })
      if (!result.data.data.getLivro)
      {
        this.Livroencontrado ='Livro não encontrado'
        this.id = ''
        return
      }
      this.Livroencontrado =''
      console.log(result.data.data.getLivro)
      this.Livro = result.data.data.getLivro
  }}
}

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
.color{
  background-color: #2c3e50;
  color: white;
}
</style>
