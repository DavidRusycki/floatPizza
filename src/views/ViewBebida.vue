<template>
  <div class="container">
    <div class="main-formulario">
      <form action="" id="Bebidas-form" @submit="createBebida">
        <div class="input-container">
          <label for="titulo">Titulo :</label>
          <input
            required
            type="text"
            name="titulo"
            id="titulo"
            v-model="titulo"
            placeholder="Digite o titulo"
          />
        </div>
        <div class="input-container">
          <label for="descricao">Valor :</label>
          <input
            required
            type="number"
            name="descricao"
            id="descricao"
            v-model="descricao"
            placeholder="Digite a descricao"
          />
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn btn btn-success" value="Adicionar Bebida" />
        </div>
      </form>

      <div>
        <ul>
          <li :key="Bebida.bebcodigo" v-for="Bebida in this.Bebidas">{{Bebida.bebdescricao}}
            <button @click="deleteBebida(Bebida)" class="btn btn-danger">Excluir</button>
          </li>
        </ul>
      </div>

    </div>
  </div>
</template>

<script>
import axios from '@/server/axios.js'

export default {
  name: 'ViewBebida',
  data () {
    return {
      titulo: null,
      descricao: null,
      Bebidas: {}
    }
  },
  methods: {
    createBebida (event) {
      event.preventDefault()
      axios
        .post('http://localhost:8080/bebidas/inserir', {
          titulo: this.titulo,
          descricao: this.descricao
        })
        .then(function (response) {
          console.log(response)
        })
        .catch(function (error) {
          console.error(error)
        })
      this.titulo = null
      this.descricao = null
    },
    deleteBebida (Bebida) {
      const indice = this.Bebidas.indexOf(Bebida)
      this.Bebidas.splice(indice, 1)

      axios
        .post('http://localhost:8080/bebidas/deletar', {
          id: Bebida.bebcodigo
        })
        .then(function (response) {
          console.log(response)
        })
        .catch(function (error) {
          console.error(error)
        })
    }
  },
  async mounted () {
    let Bebidas = await fetch('http://localhost:8080/bebidas')
    Bebidas = await Bebidas.json()
    this.Bebidas = Bebidas
    console.log(this.Bebidas)
  }
}
</script>

<style scoped>
form{
  display: flex;
  flex-direction: row;
  justify-content: center;
}
.input-container{
  margin: 1vh;
}
</style>
