<template>
  <div class="flex justify-center">
    <div class="w-98 h-100 text-center p-5 bg-green-100">
      <h1 class="text-xl font-bold py-4 my-4 shadow-md bg-pink-100 rounded-md">
        My To-Do list
      </h1>

      <!-- input -->
      <div class="flex items-center justify-between">
        <input
          v-model="task"
          type="text"
          placeholder="Enter your task here"
          class="px-4 py-2 border border-gray-400 mt-4"
        />
        <button
          class="px-2 py-2 mt-2 border bg-green-400 rounded-md"
          @click="submitTask"
        >
          Submit
        </button>
      </div>
      <div class="flex items-center justify-center">
        <table class="border-separate border-spacing-4 mt-6">
          <thead>
            <tr>
              <th class="bg-blue-300">Task</th>
              <th class="bg-blue-300">Status</th>
              <th class="bg-blue-300">Edit</th>
              <th class="bg-blue-300">Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(task, index) in tasks" :key="index">
              <td class="bg-blue-100">
                <span :class="{ finished: task.status === 'finished' }">{{
                  task.name
                }}</span>
              </td>
              <td class="bg-blue-100">
                <span
                  class="cursor-pointer"
                  :class="{
                    danger: task.status === 'to-do',
                    warning: task.status === 'in-progress',
                    success: task.status === 'finished',
                  }"
                  @click="changeStatus(index)"
                  >{{ task.status }}
                </span>
              </td>
              <td class="bg-blue-100">
                <div class="text-center" @click="editTask(index)">
                  <span class="fa fa-pen"></span>
                </div>
              </td>
              <td class="bg-blue-100">
                <div class="text-center" @click="deleteTask(index)">
                  <span class="fa fa-trash"></span>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoApp",

  data() {
    return {
      task: "",
      editedTask: null,
      availableStatus: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Solve two problems from leetcode every morning",
          status: "to-do",
        },
        {
          name: "Practice projects using vue.js ",
          status: "in-progress",
        },
        {
          name: " Revise CSS",
          status: "in-progress",
        },
      ],
    };
  },

  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do",
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
    changeStatus(index) {
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);

      if (++newIndex > 2) {
        newIndex = 0;
      }

      this.tasks[index].status = this.availableStatus[newIndex];
    },
  },
};
</script>

<style scoped>
.finished {
  text-decoration: line-through;
}
.danger {
  color: red;
}

.warning {
  color: orange;
}

.success {
  color: green;
}
</style>
