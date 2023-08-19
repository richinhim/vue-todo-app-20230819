<template>
  <div class="container" style="max-width: 600px">
    <!-- Heading -->
    <h2 class="text-center mt-5">My Vue Todo App</h2>

    <!-- Input -->
    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task"
        placeholder="Enter task"
        class="w-100 form-control"
        @keyup.enter="addTask"
      />
      <button class="btn btn-warning rounded-0" @click="addTask">
        SUBMIT
      </button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px">Status</th>
          <th scope="col" class="text-center">Delete</th>
          <th scope="col" class="text-center">Edit</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>{{ task.name }}</td>
          <td @click="modifyStatus(index)">
            <span class="pointer noselect"> {{ task.status }}</span>
          </td>
          <td class="text-center">
            <div @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
          <td class="text-center">
            <div @click="editTask(index)">
              <p class="fa fa-pen pointer"></p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      editIndex: null,
      task: "",
      isEdit: false,
      editedTask: null,
      statuses: ["to-do", "in-progress", "finished"],

      /* Status could be: 'to-do' / 'in-progress' / 'finished' */
      tasks: [
        {
          name: "Steal bananas from the supermarket.",
          status: "to-do",
        },
        {
          name: "Eat 1 kg chocolate in 1 hour.",
          status: "in-progress",
        },
        {
          name: "Create YouTube video.",
          status: "finished",
        },
      ],
    };
  },

  methods: {
    deleteTask(index) {
      console.log("index==>", index);
      this.tasks.splice(index, 1);
    },
    addTask() {
      if (this.task.length < 1) {
        alert("Enter task..");
        return;
      }
      if (!this.isEdit) {
        const newTask = {
          name: this.task,
          status: "to-do",
        };
        this.tasks.push(newTask);
        this.task = "";
      } else {
        this.tasks[this.editedIndex].name = this.task;
        this.editIndex = null;
        this.isEdit = false;
        this.task = "";
      }
    },
    modifyStatus(row) {
      this.tasks.filter((task, index) => {
        if (row === index) {
          if (task.status === "to-do") {
            this.tasks[row].status = "in-progress";
          } else if (task.status === "in-progress") {
            this.tasks[row].status = "finished";
          } else {
            this.tasks[row].status = "to-do";
          }
        }
      });
      /* this.tasks.map((task, index) => {
        if (row === index) {
          if (status === "to-do") {
            this.tasks[row].status = "in-progress";
          } else if (status === "in-progress") {
            this.tasks[row].status = "finished";
          } else {
            this.tasks[row].status = "to-do";
          }
        }
      }); */
    },
    editTask(index) {
      this.isEdit = true;
      // /console.log(index, this.tasks[index].name);
      this.task = this.tasks[index].name;
      this.editedIndex = index;
      this.editedTask = this.tasks[index];
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
.line-through {
  text-decoration: line-through;
}
</style>
