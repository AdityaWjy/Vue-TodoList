<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue Todo App</h2>

    <!-- Input -->
    <div class="d-flex">
      <input
        v-model="task"
        type="text"
        class="form-control"
        placeholder="Enter task"
      />
      <button @click="submitTask" class="btn btn-warning rounded-0">
        Submit
      </button>
    </div>

    <!-- Task Tabel-->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ finished: task.status === 'finished' }">{{
              task.name
            }}</span>
          </td>
          <td style="width: 120px">
            <span
              @click="changeStatus(index)"
              class="pointer"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-warning': task.status === 'in-progress',
                'text-success': task.status === 'finished',
              }"
              >{{ firstCharUpper(task.status) }}
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
  name: "HomeView",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      availableStatuses: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Steal bananas from the store.",
          status: "to-do",
        },

        {
          name: "Eat 1kg chocolate in 5 min",
          status: "in-progress",
        },

        {
          name: "Summon madara",
          status: "finished",
        },
      ],
    };
  },

  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      // Untuk update task
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        // Untuk submit update task
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }

      this.task = "";
    },

    // Untuk delete button
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    // Untuk edit button
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    // Untuk ganti status by index
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },

    // Kaptial diawal karakter
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}

.finished {
  text-decoration: line-through;
}
</style>
