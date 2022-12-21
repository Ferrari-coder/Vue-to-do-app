<template>
  <div class="container">
    <h2 class="tet-center mt-5">Feranmi's Vue Todo App</h2>


    <!-- the input -->
    <div class="d-flex mt-3">
      <input v-model="task" type="text" placeholder="Enter plans for the day" class="form-control">
      <button @click="addTask" class="btn btn-primary">ADD</button>
    </div>

    <!-- table for each task -->
    <table class="table mt-4">
      <thead class="thead-dark table-dark">
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td><span :class="{'finished' : task.status === 'finished'}">{{ task.name }}</span></td>
          <td>
            <span @click="changeStatus(index)" class="pointer"
              :class="{'text-danger' : task.status === 'to-do',
              'text-warning' : task.status === 'in-progress',
              'text-success' : task.status === 'finished',
            }"
            >
              {{ firstCharCaps(task.status)}}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data() {
    return {
      task: "",
      editedTask: null,
      availableStatuses: ['to-do', 'in-progress', 'finished'],

      tasks: [
        {
          name: 'finish to-do app project',
          status: 'to-do',
        },
        {
          name: 'read for 253',
          status: 'to-do',
        }
      ]
    }
  },

  methods: {
    addTask() {
      console.log("You just added a task")
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do"
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = "";
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },

    firstCharCaps(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor: pointer;
}
.finished{
  text-decoration: line-through;
}
</style>
