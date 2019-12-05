<template>
  <q-page padding>
    <!-- content -->
    <div class="row">
      <div class="col-6">
         <q-input v-model="newTask.task" placeholder="Add something to this list" />
      </div>
      <div class="col-6">
         <q-btn icon="add_circle" color="primary" @click="addTask"/>
      </div>
   </div>
   <div class="row">
    <div class="col-12">
    <q-list highlight>
      <q-list-header>CostCo List</q-list-header>
      <transition-group
  appear
  enter-active-class="animated fadeIn"
  leave-active-class="animated fadeOut"
>
      <q-item v-for="item in availableItems" :key="item.id">
         <q-item-main :label="item.task" :class="{'text-weight-light': item.staged}"></q-item-main>
         <q-item-side right>
            <q-btn round color="primary" icon="check_circle" @click="checkBox(item.id)" />
         </q-item-side>
      </q-item>
   </transition-group>
    </q-list>
  </div>
  </div>

  </q-page>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      items: [
        { id: 0, task: 'This is the first task', completed: false, owner: 'Andy' },
        { id: 1, task: 'This is the second task', completed: false, owner: 'Amanda' },
        { id: 2, task: 'This is the third task', completed: false, owner: 'Andy' }
      ],
      newTask: {
        task: '',
        completed: false,
        owner: 'Andy',
        id: 4
      }
    }
  },
  computed: {
    availableItems: function () {
      // returns an array of tasks that are not completed
      return this.items.filter(obj => {
        if (obj.completed === false) {
          return obj
        }
      })
    },
    nextTaskId: function () {
      return Math.max.apply(Math, this.items.map(function (o) { return o.id }))
    }
  },
  methods: {
    addTask: function () {
      let task = this.newTask
      task.id = this.nextTaskId + 1
      this.items.push(Object.assign({}, task))
      this.newTask.task = ''
    },
    removeTask: function (key) {
      this.items.splice(key, 1)
    },
    checkBox: function (key) {
      // cross out the task and fade the text
      this.items[key].staged = true
      window.setTimeout(this.completeTask(key), 5000)
    },
    completeTask: function (key) {
      this.items[key].completed = true
    }
  }
}
</script>

<style>
</style>
