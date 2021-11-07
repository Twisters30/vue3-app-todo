<template>
  <li v-for="task in tasks" :key="task.id" class="task__item collection-item avatar">
      <img src="~@/assets/icons/notepad.png" alt="" class="task__img circle">
      <span class="task__name title">{{ task.name }}</span>
      <p class="task__description">
        {{ task.description }}
      </p>
      <label class="task__checkbox-wrapp secondary-content">
        <div>
          <input v-model="task.status" @click="taskState(task.id)" type="checkbox" class="filled-in" />
          <span :class="{'in-progress': !task.status}">{{ task.status ? 'Done': 'in progress' }}</span>
        </div>
      </label>
    <button @click="deleteTask(task.id)" class="task__btn-delete waves-light btn-small red">delete</button>
    <button @click="editTask(task.id)" class="task__btn-edit waves-light btn-small green">edit</button>
  </li>
</template>

<script>

import M from 'materialize-css'
export default {
  name: 'TaskItem',
  props: {
    arrayTasks: Array,
    transformID: {
      type: Boolean,
      default () {
        return false
      }
    }
  },
  data () {
    return {
      forceUpdate: this.transformID,
      editTasksArr: [],
      tasks: this.arrayTasks
    }
  },
  watch: {
    arrayTasks (value) {
      this.tasks = value
    },
    forceUpdate (value) {
      console.log(value)
      if (!value) {
        console.log(this.tasks[0].id)
        this.tasks[0].id += 1
      }
    }
  },
  mounted () {
    setTimeout(() => {
      M.updateTextFields()
    }, 0)
  },
  methods: {
    editTask (id) {
      this.$emit('sendId', id)
    },
    deleteTask (id) {
      console.log(this.tasks)
      this.tasks.forEach((el, i) => {
        if (el.id === id) {
          this.tasks.splice(i, 1)
          localStorage.setItem('tasks', JSON.stringify(this.tasks))
        }
      })
    },
    taskState (id) {
      this.tasks.forEach((el, i) => {
        if (el.id === id) {
          this.tasks[i].status = !this.tasks[i].status
          localStorage.setItem('tasks', JSON.stringify(this.tasks))
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.task {
  &__item {
    position: relative;
  }
  &__description {
    max-width: 85%;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  &__btn-delete {
    display: block;
    position: absolute;
    bottom: 5px;
    right: 5px;
  }
  &__btn-edit {
    display: block;
    position: absolute;
    bottom: 5px;
    right: 90px;
  }
  &__name {
    display: block;
    padding-bottom: 5px;
    font-size: 21px !important;
  }
  &__checkbox-wrapp {
    min-width: 110px;
  }
  &__wrapp-input {
    position: relative;
  }
}
.circle {
  border-radius: 0;
}
.in-progress {
  color: #ffab00;
}
</style>
