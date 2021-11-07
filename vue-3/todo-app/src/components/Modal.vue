<template>
    <div v-show="modalId" class="modal__content">
      <div class="row">
        <form class="col s12">
          <div class="row">
            <div class="input-field col s6">
              <input v-model="singleTask[0].name" class="validate" id="input_text" type="text" data-length="10" required>
              <label for="input_text">Task name</label>
              <span class="helper-text" data-error="min length 10 chars"></span>
            </div>
          </div>
          <div class="row">
            <div class="modal__description input-field col s6">
              <label for="textarea2">Date pick</label>
              <input v-model="singleTask[0].date" ref="datepickerModal" type="text" class="datepicker validate" required>
              <span class="helper-text" data-error="is required"></span>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
          <textarea
            v-model="singleTask[0].description" id="textarea2"
            ref="textArea"
            :class="{'invalid':singleTask[0].description.length > 2048}"
            class="materialize-textarea">
          </textarea>
              <label for="textarea2">Task description</label>
              <span
                :class="{'error-char':singleTask[0].description.length > 2048}"
                class="character-counter"
                style="float: right; font-size: 12px;"
              >
            {{ characterCounter }}/2048
          </span>
            </div>
          </div>
          <button @click.prevent="saveTask" class="btn" type="submit" name="action">
            Save
          </button>
        </form>
      </div>
    </div>
</template>

<script>
import 'materialize-css/dist/js/materialize.min'
import M from 'materialize-css'
export default {
  name: 'Modal',
  props: {
    currentId: Number
  },
  data () {
    return {
      tasks: [],
      date: '',
      modalId: null,
      singleTask: [{
        name: '',
        date: null,
        description: ''
      }]
    }
  },
  watch: {
    currentId (value) {
      this.modalId = value
      this.findCurrentTask()
      this.singleTask.date = M.Datepicker.init(this.$refs.datepickerModal, {
        format: 'dd.mm.yyyy'
      }
      )
      setTimeout(() => {
        M.updateTextFields()
      }, 0)
    }
  },
  computed: {
    characterCounter () {
      return this.singleTask[0].description.length > 2048 ? 'error' : this.singleTask[0].description.length
    }
  },
  methods: {
    updateLanding () {
      this.$router.go()
    },
    getDate () {
      const value = this.$refs.datepickerModal.value
      return value
    },
    formatDate (date) {
      return new Date(date).toLocaleDateString()
    },
    getTasks () {
      this.tasks = JSON.parse(localStorage.getItem('tasks'))
    },
    findCurrentTask () {
      this.getTasks()
      this.singleTask = this.tasks.filter(task => task.id === this.modalId)
      // this.singleTask[0].date = this.formatDate(this.singleTask[0].date)
      this.$refs.textArea.value = this.singleTask[0].description
      M.textareaAutoResize(this.$refs.textArea)
    },
    saveTask () {
      this.tasks.forEach((el, i) => {
        if (el.id === this.singleTask[0].id) {
          this.tasks[i] = this.singleTask[0]
          this.tasks[i].date = this.date.date ? this.formatDate(this.date.date) : this.getDate()
        }
      })
      this.sendToStorage(this.tasks)
      this.updateLanding()
    },
    sendToStorage (task) {
      localStorage.setItem('tasks', JSON.stringify(task))
    }
  }
}
</script>

<style lang="scss" scoped>

.modal {
  &__content {
    padding: 20px 0;
  }
}

.error-char {
  color: #ef5350;
}
</style>
