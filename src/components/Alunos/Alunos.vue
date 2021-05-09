<template>
  <div id="alunos">
    <Titulo titulo="Alunos"/>
    <div id="form">
      <input type="text" v-model="aluno" @keyup.enter="addAluno()" placeholder="Digite o nome do Aluno"/>
      <button @click="addAluno()">Adicionar</button>
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>Matricula</th>
          <th>Nome</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody v-if="alunos.length">
        <tr v-for="aluno in alunos" :key="aluno.id">
          <td>{{ aluno.id }}</td>
          <td>{{ aluno.name }}</td>
          <td>
            <button @click="deletarAluno(aluno.id)">Remover</button>
          </td>
        </tr>
      </tbody>
      <tfoot v-else>
        <tr>
          <td colspan="3">Nenhum Aluno Cadastrado</td>
        </tr>
      </tfoot>
    </table> 
  </div>
</template>

<script>
import Axios from 'axios';
import Titulo from '../_share/Titulo';


export default {
  name: 'Alunos',
  components: {
    Titulo
  },
  data() {
    return {
      title: 'Alunos',
      aluno: '',
      alunos: [],
    }
  },
  created() {
    this.getAlunos();
  },
  methods: {
    addAluno() {
      if (this.aluno) {
        let aluno = {
          name: this.aluno
        }
        this.createAluno(aluno);
      }
    },
    deletarAluno(indice) {
      Axios.delete(`http://localhost:3000/alunos/${indice}`)
        .then(() => {
          this.getAlunos();
        })
        .catch(error => {
          console.log(error)
        });
    },
    getAlunos() {
      Axios.get('http://localhost:3000/alunos')
      .then(data => this.alunos = data.data)
      .catch(error => console.log(error));
    },
    createAluno(data) {
      
      Axios.post('http://localhost:3000/alunos', data)
      .then(() => {
        this.aluno = '';
        this.getAlunos();
      })
      .catch(error => console.log(error));
    },
  }
}
</script>

<style scoped>
  #alunos {
    display: flex;
    flex-direction: column;
    padding: 0 30%;
  }

  #alunos h2 {
    text-align: center;
  }

  #alunos #form {
    display: flex;
    flex-direction: row;
    justify-content: center;
    margin: 10px 0;
  }

  #alunos #form input {
    width: 85%;
    padding: 10px;
  }

  #alunos #form button {
    border: none;
    background-color: rgb(6, 133, 90);
    color: #FFF;
    margin: 0 0 0 5px;
    cursor: pointer;
    border-radius: 5px;
  }

  #alunos .table {
    text-align: center;
  }

  #alunos .table thead {
    background-color: rgb(5, 96, 201);
    color: #FFF;
  }

  #alunos .table tbody {
    background-color: rgb(215, 215, 216);
    color: rgb(85, 84, 84);
    font-size: 1.2rem;
  }

  #alunos .table thead tr th, #alunos .table tbody tr td {
    padding: 10px;
  }

  #alunos .table tbody tr td button {
    padding: 8px;
    background-color: rgb(161, 1, 1);
    color: #FFF;
    border: none;
    cursor: pointer;
    border-radius: 5px;
  }

  #alunos .table tbody tr td button:hover {
    background-color: rgb(119, 4, 4);
    
  }

  #alunos .table tfoot {
    background-color: rgb(215, 215, 216);
    color: rgb(85, 84, 84);
    font-size: 1.2rem;
  }

  #alunos .table tfoot tr td {
    padding: 10px;
  }

  @media(max-width: 768px) {
    #alunos {
      padding: 0 10%;
    }
  }
</style>
