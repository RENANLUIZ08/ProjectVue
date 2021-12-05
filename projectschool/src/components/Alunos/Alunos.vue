<template>
  <div>
    <titulo texto="Aluno"/>
    <div class="input-group col-md-4">
      <input type="text" class="form-control" placeholder="Informe o nome do Aluno" aria-label="Informe o nome do Aluno" aria-describedby="basic-addon2" v-model="nome" v-on:keyup.enter="addAluno()">
      <div class="input-group-append">
        <button class="btn btn-outline-success" type="button" v-on:click="addAluno()">Incluir aluno <i class="fas fa-user-plus"></i></button>
      </div>
    </div>
    <br>
  <table class="table">
    <thead>
      <th>Matricula</th>
      <th>Nome</th>
      <th>Sobrenome</th>
      <th>Idade</th>
      <th>Opções</th>
    </thead>
    <tbody v-if="alunos.length">
      <tr v-for="(aluno, index) in alunos" :key="index">
        <!-- <td>{{aluno.id}}</td> -->
        <td>{{aluno.id}}</td>
        <td>{{aluno.Nome}}</td>
        <td>{{aluno.Sobrenome}}</td>
        <td>{{aluno.Idade}}</td>
        <td><button class="btn btn-danger" @click="remAluno(aluno)">Remover <i class="fas fa-user-times"></i></button></td>
      </tr>
    </tbody>
    <tfoot v-else>
      Não existem alunos
    </tfoot>
  </table>
  </div>
</template>

<script>
import Titulo from '../_share/Titulo';

export default {
  components: {
    Titulo
  },
  data(){
    return {
      titulo: "Aluno",
      nome: "",
      alunos: []
    }
  },
  created() {
    this.$http.get('http://localhost:3000/alunos')
    .then(res => res.json())
    .then(alunos => this.alunos = alunos)
  },
  props: {
  },
  methods: {
    addAluno() {
      let _aluno = {
        Nome: this.nome,
        Sobrenome: "",
        Idade: "29 Anos"        
      }

    this.$http.post('http://localhost:3000/alunos', _aluno)
    .then(res => res.json())
    .then(novoAluno => {
      this.alunos.push(novoAluno);
            this.nome = '';
    })
      // this.alunos.forEach(aluno => {
      //   console.log(aluno);
      // });
    },
    remAluno(aluno){

    this.$http.delete(`http://localhost:3000/alunos/${aluno.id}`,)
    .then(() => {
      let indice = this.alunos.indexOf(aluno);
      this.alunos.splice(indice, 1);      
    })

    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
