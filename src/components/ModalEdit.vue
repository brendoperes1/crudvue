<template>
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">

          <div v-if="isNew">  
            <div class="modal-header">
              <slot name="header">
                Adicionar Usuário
              </slot>
              <button @click="$emit('close')" type="button" class="inline-flex items-center justify-center rounded-md border border-transparent 
              bg-indigo-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 
              focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 sm:w-auto" style="float: right;">
                X
              </button>
            </div>  

            <div class="modal-body">
              <slot name="body">
                <div class="input-container">
                  <input class="input" v-model="name" placeholder="Nome" />
                  <input class="input" v-model="company" placeholder="Empresa" />
                  <tag-input
                  @phoneTag="phoneTag"/>
                  <input type="email" class="input" v-model="email" placeholder="E-mail" />
                </div>
              </slot>
            </div>

            <div class="modal-footer">
              <slot name="footer">
                <button @click="add" type="button" class="inline-flex items-center justify-center rounded-md border border-transparent 
                  bg-indigo-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 
                  focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 sm:w-auto" style="float: right;">
                    Salvar
                </button>
              </slot>
            </div>
          </div>

          <div v-if="!isNew">
            <div class="modal-header">
              <slot name="header">
                Editar Usuário
              </slot>
              <button @click="$emit('close')" type="button" class="inline-flex items-center justify-center rounded-md border border-transparent 
              bg-indigo-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 
              focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 sm:w-auto" style="float: right;">
                X
              </button>
            </div>  

            <div class="modal-body">
              <slot name="body">
                <div class="input-container">
                  <span>{{user.phone}}</span>
                  <input class="input" v-model="user.name" placeholder="Nome" />
                  <input class="input" v-model="user.company" placeholder="Empresa" />
                  <tag-input
                  @hasPhone="user"
                  @phoneTag="phoneTag"/>
                  <input class="input" v-model="user.email" placeholder="E-mail" />
                </div>
              </slot>
            </div>

            <div class="modal-footer">
              <slot name="footer">
                <button @click="$emit('close')" type="button" class="inline-flex items-center justify-center rounded-md border border-transparent 
                  bg-indigo-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 
                  focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 sm:w-auto" style="float: right;">
                    Salvar
                </button>
              </slot>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import TagInput from './TagInput.vue'
export default {
  components: { TagInput },
    props: {
        isNew: {
          type: Boolean,
          default: false
        },
        hasUser: {
          type: Object
        }
    },
    data() {
        return {
          user: this.hasUser,
          name: '',
          company:'',
          email:'',
          phone: []
        }
    },
    methods: {
        phoneTag(phone){
          this.phone = phone;
        },
        add() {
            this.$emit('userAdded', { 
              name: this.name,
              company: this.company,
              email: this.email,
              phone: this.phone
            });
            this.name = '',
            this.company = '',
            this.email = '',
            this.phone = ''

            this.$emit('close');
        }
    }
}
</script>


<style scoped>
.modal-header {
  text-align: center;
}

.input-container {
  display: flex;
  flex-direction: column;
}
.input {
  margin-bottom: 20px;
  padding: 12px 20px;
  border: 2px solid;
  border-radius: 8px;
}
input:focus {
  outline-color: #3949AB;
}

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: auto;
  margin: 0px 40vh;
  padding: 39px 30px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  padding-bottom: 10px;
}

.modal-footer {
  text-align: right;
  padding-bottom: 10px;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter-from, .modal-leave-to {
  opacity: 0;
}

.modal-enter-active .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>