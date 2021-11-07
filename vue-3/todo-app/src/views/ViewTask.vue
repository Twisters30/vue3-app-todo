<template>
  <div v-show="isActive" v-if="tasks.length" class="content">
    <div class="input-field">
      <select v-model="filters" ref="select">
        <option value="all">all</option>
        <option value="done">done</option>
        <option value="progress">progress</option>
      </select>
      <label>Sort</label>
    </div>
    <ul class="collection">
      <TaskItem
        @sendId="catchId"
        :array-tasks="filteredTasks"
        :transform-id="isActive"
      />
    </ul>
  </div>
  <div v-if="!tasks.length" class="content__center">
    <h2>
      Задач пока что нет!
    </h2>
    <button @click="$router.push('/create')" class="waves-effect waves-light btn-large" type="button">
      <i class="material-icons dp48 right">event_note</i>Создать задачу?
    </button>
  </div>
  <Modal
    @sendStateEdit="catchStateEdit"
    :current-id="transferId"
  />
</template>

<script>
import Modal from '../components/Modal'
import 'materialize-css/dist/js/materialize.min'
import M from 'materialize-css'
import TaskItem from '../components/TaskItem'
export default {

  data () {
    return {
      filters: 'all',
      tasks: [],
      tasksForModal: [],
      isActive: true,
      transferId: null
    }
  },
  components: { TaskItem, Modal },
  mounted () {
    this.getTasks()
    this.filters = M.FormSelect.init(this.$refs.select, {}
    )
  },
  methods: {
    forceUpdate () {
      if (this.tasks) {
        this.tasks[0].id += 1
      }
    },
    catchStateEdit (state) {
      this.isActive = state
      this.forceUpdate()
    },
    catchId (id) {
      this.transferId = id
      this.isActive = !this.isActive
    },
    getTasks () {
      this.tasks = JSON.parse(localStorage.getItem('tasks'))
    }
  },
  computed: {
    filteredTasks () {
      if (this.filters === 'all') {
        return this.tasks
      }
      if (this.filters === 'done') {
        return this.tasks.filter(task => task.status)
      }
      if (this.filters === 'progress') {
        return this.tasks.filter(task => !task.status)
      }
      return this.tasks
    }
  }
}
</script>
<style lang="scss" scoped>
.collection {
  border: none;
  -webkit-box-shadow: 0px 0px 10px 2px rgba(34, 60, 80, 0.44);
  -moz-box-shadow: 0px 0px 10px 2px rgba(34, 60, 80, 0.44);
  box-shadow: 0px 0px 10px 2px rgba(34, 60, 80, 0.44);
}
.input-field {
  width: 20%;
}
.content {
  padding: 20px 0;
  &__center {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
}
</style>
