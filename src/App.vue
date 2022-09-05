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
              <input type="submit" value="Adicionar"/>
              <br/>
              {{nome}} - {{duracao}}
          </form>
          <br/>
          <input type="text" v-model="filtro" placeholder="Filtro"/>
          <button @click="ordenar()">Ordenar</button>
          <table>
            <tr>
              <th>Id</th>
              <th>Nome</th>
              <th>Duração</th>
              <th></th>
            </tr>
            <tr v-for="p in projetosFiltrados" :key="p.id" @click="selected = p"
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
    salvar() {
      const achou = this.projetos.some(p => p.nome == this.nome)  
      if (achou) {
        alert('achou')
      }
      else {
        
        this.projetos.push(
          {
            id: this.getMaiorId(),
            nome: this.nome,
            duracao: this.duracao
          }
        )
        this.nome = ''
        this.duracao = null
        this.mostraFormulario = false
      }

      
      /* 
      let achou = false
      for(let i = 0; i < this.projetos.length; i++) {
        console.log(this.projetos[i].nome == this.nome)
         if (this.projetos[i].nome == this.nome) {
            achou = true
         }
      }
      if (achou) {
        alert('achou')
      }
      else {
        this.projetos.push(
          {
            nome: this.nome,
            duracao: this.duracao
          }
        )
      }*/


      //this.projetos.forEach((p, idx) => console.log(idx + " " +p.nome))
      //alert('mensagem')

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

.selected {
  background-color: pink;
}

</style>
