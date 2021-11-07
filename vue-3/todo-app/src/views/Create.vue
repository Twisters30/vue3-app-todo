<template>
  <div class="row">
    <form @submit.prevent="createTask" class="col s12">
      <div class="row">
        <div class="input-field col s6">
          <input v-model="task.name" class="validate" id="input_text" type="text">
          <label for="input_text">Task name</label>
          <span v-if="v$.task.name.$error" class="red-text">Заполните поле</span>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s6">
          <label for="textarea2">Date pick</label>
          <input v-model="task.date" ref="datepicker" type="text" class="datepicker validate">
          <span v-if="v$.task.date.$error" class="red-text">Неверно узазана дата</span>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <textarea
            v-model="task.description" id="textarea2"
            :class="{'invalid':task.description.length > 2048}"
            class="materialize-textarea">
          </textarea>
          <label for="textarea2">Task description</label>
          <span
            :class="{'error-char':task.description.length > 2048}"
            class="character-counter"
            style="float: right; font-size: 12px;"
          >
            {{ characterCounter }}/2048
          </span>
          <span class="red-text" v-if="v$.task.description.$error">Много букв</span>
        </div>
      </div>
      <button class="btn" type="submit" name="action">
        Create task
        <i class="material-icons right">send</i>
      </button>
    </form>
  </div>
</template>

<script>

import useValidate from '@vuelidate/core'
import { required, maxLength, minLength } from '@vuelidate/validators'
import 'materialize-css/dist/js/materialize.min'
import M from 'materialize-css'
export default {
  name: 'Create',
  setup () {
    return {
      v$: useValidate()
    }
  },
  data () {
    return {
      tasks: [],
      task: {
        name: '',
        date: null,
        description: ''
      }
    }
  },
  validations () {
    return {
      task: {
        name: {
          required
        },
        description: {
          min: maxLength(10)
        },
        date: {
          required,
          min: minLength(10)
        }
      }
    }
  },
  mounted () {
    this.task.date = M.Datepicker.init(this.$refs.datepicker, {
      format: 'dd.mm.yyyy',
      defaultDate: new Date(),
      setDefaultDate: true
    }
    )
    this.update()
  },
  computed: {
    characterCounter () {
      return this.task.description.length > 2048 ? 'error' : this.task.description.length
    }
  },
  methods: {
    clearInputs () {
      this.task.name = ''
      this.task.description = ''
    },
    update () {
      this.tasks = JSON.parse(localStorage.getItem('tasks') || '[]')
      console.log(this.tasks)
    },
    createTask () {
      const task = {
        name: this.task.name,
        description: this.task.description,
        date: this.task.date.date,
        id: Date.now(),
        status: false
      }
      this.tasks.push(task)
      this.sendToStorage(this.tasks)
    },
    sendToStorage (task) {
      this.v$.$validate()
      if (this.v$.task.date.$error) {
        console.log('datepicker', this.v$.task.date.$error)
        const instance = M.Datepicker.init(this.$refs.datepicker, {
          format: 'dd.mm.yyyy',
          defaultDate: new Date(),
          setDefaultDate: true
        })
        instance.open()
      }
      if (!this.v$.$error) {
        localStorage.setItem('tasks', JSON.stringify(task))
        this.clearInputs()
      } else {
        console.log('Error')
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.error-char {
  color: #ef5350;
}
</style>
