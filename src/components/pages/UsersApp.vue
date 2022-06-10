<template>

  <div class="px-4 sm:px-6 lg:px-8">
    <div class="sm:flex sm:items-center">
      <div class="sm:flex-auto">
        <h1 class="text-xl font-semibold text-gray-900">Usuários</h1>
        <p class="mt-2 text-sm text-gray-700">Lista de usuários cadastrados no sistema.</p>
      </div>
      <div class="mt-4 sm:mt-0 sm:ml-16 sm:flex-none">
        <button @click="modalOpenNew" type="button" class="inline-flex items-center justify-center rounded-md border border-transparent 
        bg-indigo-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 
        focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 sm:w-auto">
          Novo usuário
        </button>
      </div>
    </div>
    <div class="mt-8 flex flex-col">
      <div class="-my-2 -mx-4 overflow-x-auto sm:-mx-6 lg:-mx-8">
        <div class="inline-block min-w-full py-2 align-middle md:px-6 lg:px-8">
          <div class="overflow-hidden shadow ring-1 ring-black ring-opacity-5 md:rounded-lg">
            <table class="min-w-full divide-y divide-gray-300">
              <thead class="bg-gray-50">
                <tr>
                  <th scope="col" class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-6">Nome</th>
                  <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Telefone</th>
                  <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Empresa</th>
                  <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">E-mail</th>
                  <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Data de cadastro</th>
                  <th scope="col" class="relative py-3.5 pl-3 pr-4 sm:pr-6">
                    <span class="sr-only">Edit</span>
                  </th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-200 bg-white">
                <!-- poderia colocar index aqui no v-for -->
                <tr v-for="person in users" :key="person.email">
                  <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6">{{ person.name }}</td>
                  <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ person.phone }}</td>
                  <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ person.company }}</td>
                  <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ person.email }}</td>
                  <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ person.data }}</td>
                  <td class="relative whitespace-nowrap py-4 pl-3 pr-4 text-right text-sm font-medium sm:pr-6">
                  <div style="display:flex">  
                    <div style="width:20px; margin-right:10px">
                      <PencilIcon @click="modalOpen(person)"/>
                    </div>
                    <div style="width:20px">
                      <XCircleIcon style="color:red" @click="deleteUser(person)"/>
                    </div>
                  </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
    <modal-edit v-if="showModal" 
      @close="showModal=false" 
      @userAdded="addUser"
      :isNew="isNew"
      :hasUser="hasUser"/>
  </div>

</template>


<script>
/* eslint-disable */
import ModalEdit from '../ModalEdit.vue'
import {
  PencilIcon,
  XCircleIcon,
} from '@heroicons/vue/outline'

export default {   
  name: 'UsersApp', 
  components: { ModalEdit, PencilIcon, XCircleIcon },
  data() {
    return {
      users : [
          { name: 'Fulano da Silva', company: 'E-Inov Soluções Tecnológicas', email: 'fulano.silva@example.com', data: '2022-01-05' },
          { name: 'Fulano da Silva 2', company: 'E-Inov Soluções Tecnológicas', email: 'fulano.silva2@example.com', data: '2022-01-16' },
          { name: 'Fulano da Silva 3', company: 'E-Inov Soluções Tecnológicas', email: 'fulano.silva3@example.com', data: '2022-01-27' },
          { name: 'Fulano da Silva 4', company: '', email: 'fulano.silva4@example.com', data: '2022-02-09' },
          { name: 'Beltrano da Silva', company: '', email: 'beltrano.silva@example.com', data: '2022-04-27' },
          { name: 'Beltrano da Silva 2', company: 'Guest Posts', email: 'beltrano.silva2@example.com', data: '2022-04-29' },
          { name: 'Beltrano da Silva 3', company: 'Guest Posts', email: 'beltrano.silva3@example.com', data: '2022-05-02' },
          { name: 'Beltrano da Silva 4', company: 'Guest Posts', email: 'beltrano.silva4@example.com', data: '2022-05-27' },
        ],
      showModal: false,
      isNew: false,
      hasUser: null,
    }
  },
  methods: {
    //cadastra com a data de hoje, ou seja, data que foi realizado o cadastro
    getDate() {
      let today = new Date();
      let dd = String(today.getDate()).padStart(2, '0');
      let mm = String(today.getMonth() + 1).padStart(2, '0'); 
      let yyyy = today.getFullYear();

      today = yyyy + '-' + mm + '-' + dd;
      return today;
    },
    addUser(user) {
      let pPhone = Object.values(user.phone)[0][0];
			const sameName = t => t.name === user.name
      //verificação pra ver se o mesmo nome já foi cadastrado, poderia ser feito pelo email já que uso ele como chave
			const reallyNew = this.users.filter(sameName).length == 0
      let date = this.getDate();
			if(reallyNew) {
				this.users.push({
					name: user.name,
          phone: pPhone,
          company: user.company,
          email: user.email,
          data: date,
					pending: user.pending || true
				})
			}
      this.showModal=false;
		},
		deleteUser(i) {
      let count = 0;
      let aux = this.users;
      //encontrar o index
      aux.forEach(function (value, index) {
        //verifica se o nome selecionado é igual o email cadastrado (chave que utilizei ao inves de ID)
        if(i.email === value.email) {
          count = index;
        } else {
          // console.log('diferente');
        }
      });
      //remover o elemento de acordo com seu index
      this.users.splice(count, 1);
		},
    modalOpenNew() {
      this.isNew = true;
      this.showModal = !this.showModal;
    },
    modalOpen(person) {
      this.isNew = false;
      this.hasUser = person;
      this.showModal = !this.showModal;
    }
  }
}

</script>