<template>
  <q-page class="flex flex-center">
    <div>
      <q-card class="my-card">
        <div class="title-cadastro">
          <q-card-section class="bg-primary text-white">
            <div class="text-h6">Cadastro de Usuário</div>
          </q-card-section>
        </div>
        <div class="inputs">
          <q-input v-model="user.name" placeholder="Informe o Nome"/>
          <q-input v-model="user.age" placeholder="Informe a Idade"/>
          <q-input v-model="user.email" placeholder="Informe o Email"/>
        </div>
        <div>
          <div class="buttons" align="center">
            <q-btn v-if="!isEdit" @click="saveUser(user)" flat>Cadastrar</q-btn>
            <q-btn v-if="isEdit" @click="updateUser(user)" flat>Atualizar Cadastro</q-btn>
            <q-btn flat>Cancelar</q-btn>
          </div>
        </div>
      </q-card>
      <div style="margin-top:10px" class="title-cadastro">
        <q-card-section class="bg-primary text-white">
          <div class="text-h6">Lista de Usuários</div>
        </q-card-section>
      </div>
      <div v-for="(contato, index) in users" :key="index">
        <q-card flat bordered class="my-card-user">
          <q-card-section>
            <div class="text">
              <span><strong>Nome: </strong>{{ contato.name }}</span> <br />
              <span><strong> Idade: </strong>{{ contato.age}} anos</span>
              <span><strong >Email: </strong>{{contato.email}}</span>
              <q-btn class="buttons-user" @click="removeUser(user.id)" flat>Deletar</q-btn>
              <q-btn class="buttons-user" v-if="!isEdit" @click.prevent="editUser(user)" flat>Editar</q-btn>
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<style >
.my-card {
  width: 600px;
  height: 400px
}
.my-card-user {
  margin-top: 20px;
  width: 600px;
  height: 80px;
}
.my-card-user p {
  font-size: 0.8rem;
}
.text {
  display: inline;
  justify-content: center;
  align-items: center;
}
.text span {
  margin-right: 5px;
}
.buttons-user {
  margin-top: -7px;
  float: right;
  width: 70px;
}
.title-cadastro {
  text-align: center;
}
.buttons {
  margin-top: 50px;
}
.inputs {
  width: 400px;
  margin: 50px auto;
}
</style>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      users: [],
      user: {
        id: '',
        name: '',
        age: '',
        email: ''
      },
      isEdit: false
    }
  },
  created () {
    this.users = JSON.parse(localStorage.getItem('users'))
  },
  methods: {
    saveUser (user) {
      let users = localStorage.getItem('users')

      user.id = new Date().getTime()

      if (users) {
        users = JSON.parse(users)
        users.push(user)
      } else {
        users = [user]
      }

      this.users = users

      localStorage.setItem('users', JSON.stringify(users))
    },
    editUser (user) {
      this.user = user
      this.isEdit = true
    },
    updateUser (user) {
      const users = this.users.map(userMap => {
        if (userMap.id === user.id) {
          return user
        }
        return userMap
      })

      this.users = users
      this.isEdit = false
      localStorage.setItem('users', JSON.stringify(users))
    },
    removeUser (userId) {
      let users = localStorage.getItem('users')

      if (!users) return

      users = JSON.parse(users)

      users = users.filter((user) => {
        return user.id !== userId
      })

      this.users = users

      localStorage.setItem('users', JSON.stringify(users))
    }
  }
}
</script>
