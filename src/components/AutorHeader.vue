<template>
    <div>
        <p>Autor: {{ autor }}</p>
        <p :title="mensagem">{{ new Date().toLocaleString() }}</p>
        <button @click="transformar()">Mudar autor</button>
    </div>
</template>

<script>
export default {
    props: {
      nome: String
    },
    data() {
        return {
          autor: null,
          mensagem: 'Meu primeiro App vue',
          maiusculaMinuscula: false
        }
    },
    methods: {
      transformar() {
        this.autor = this.maiusculaMinuscula 
                      ? this.autor.toUpperCase()
                      : this.autor.toLowerCase()
        this.maiusculaMinuscula = !this.maiusculaMinuscula
      }
    },
    mounted() {
      this.autor = this.nome
      console.log('mounted')
    },
    watch: {
      nome: function(value) {
        this.autor = value
      }
    }
}
</script>

<style>

</style>