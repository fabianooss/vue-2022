<template>
  <div>
      <p>Autor: {{ autor }}</p>
      <p :title="mensagem">{{ new Date().toLocaleString() }}</p>
      <button @click="transformar()">Mudar autor</button>

      <div>
          <h2>Projetos</h2>  
          <button @click="mostraFormulario = true" v-show="!mostraFormulario">Formulário</button>
          <form @submit.prevent="salvar()" v-if="mostraFormulario">
              <label for="nome">Nome</label>
              <input type="text" id="nome" v-model="nome"  size="30" required autofocus><br/>
              <label for="duracao">Duração</label>
              <input type="number" id="duracao" v-model="duracao" required><br/>
              <input type="submit" :value="textoBotaoSalvar"/>
              <br/>
          </form>
          <br/>
          <input type="text" v-model="filtro" placeholder="Filtro"/>
          <button @click="ordenar()">Ordenar</button>
          <table class="table">
            <tr>
              <th>Id</th>
              <th>Nome</th>
              <th>Duração</th>
              <th></th>
            </tr>
            <tr v-for="p in projetosFiltrados" :key="p.id" @click="selecionar(p)"
                                              :class="{'selected' : isSelected(p) }"
                >
                <td>{{ p.id }}</td>
                <td>{{ p.nome }}</td>
                <td>{{ p.duracao }}</td>
                <td><a href="#" @click="excluir(p)">Excluir</a></td>
            </tr>
            
          </table>

      </div>

  </div>
  
</template>

<script>

export default {

  data() {
    return {
      // json javascript object notation
      autor: 'Fabiano Oss',
      mensagem: 'Meu primeiro App vue',
      maiusculaMinuscula: false,
      filtro: '',
      mostraFormulario: false,
      selected: null,
      projetos: [
        {
          id: 15,
          nome: 'Projeto Java',
          duracao: 10
        },
        {
          id: 3,
          nome: 'Projeto Delphi',
          duracao: 11
        },
        {
          id: 7,
          nome: 'Projeto Python',
          duracao: 12
        }
      ],
      nome: '',
      duracao: null
    }
  },

  methods: {
    selecionar(projeto) {
      this.selected = projeto
      this.mostraFormulario = true
      this.nome = projeto.nome
      this.duracao = projeto.duracao
    },
    ordenar() {
      this.projetos.sort( (a,b) => b.nome.localeCompare(a.nome) ) 
    },
    isSelected(projeto) {
      return this.selected != null && this.selected.id == projeto.id
    },

    transformar() {
      this.autor = this.maiusculaMinuscula 
                      ? this.autor.toUpperCase()
                      : this.autor.toLowerCase()
      this.maiusculaMinuscula = !this.maiusculaMinuscula
       
    },
    getMaiorId() {
        //Math.max(...this.projetos.map( p => p.id))

        return this.projetos.reduce( (a,b) => a.id > b.id ? a : b, 0 ).id + 1
        // let maiorId = 0
        // this.projetos.forEach(p => maiorId = p.id > maiorId ? p.id : maiorId)
        // return ++maiorId

    },
    validar() {
      const achou = this.projetos
              .some(p => p != this.selected && p.nome == this.nome)  
      if (achou) {
        alert('Projeto já existe')
        return false
      }
      return true

    },
    inserir() {
      if (!this.validar()) 
        return false;
      this.projetos.push(
        {
          id: this.getMaiorId(),
          nome: this.nome,
          duracao: this.duracao
        }
      )
      return true
    },
    alterar() {
      if (!this.validar()) 
        return false;
      this.selected.nome = this.nome
      this.selected.duracao = this.duracao
      return true
    },
    salvar() {
      let sucesso = this.selected == null ? this.inserir() : this.alterar();
      if (sucesso)
        this.limparCampos()
    },
    limparCampos() {
      this.selected = null
      this.nome = ''
      this.duracao = null
      this.mostraFormulario = false      
    },
    excluir(projeto) {
      this.projetos.splice(this.projetos.indexOf(projeto), 1)
    }
  },
  computed: {
    projetosFiltrados() {
      if (this.filtro.trim().length == 0)
        return this.projetos
      return this.projetos.filter(p => p.nome.toLowerCase()
                .includes(this.filtro.toLowerCase()))
    },
    textoBotaoSalvar() {
      return this.selected == null ? "Adicionar" : "Alterar"
    }
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
  margin-top: 60px;
}
.table {
  cursor: pointer;
}

.selected {
  background-color: pink;
}

</style>
