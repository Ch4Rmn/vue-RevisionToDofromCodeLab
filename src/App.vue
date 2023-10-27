<template>
  <div class="container">
    <!-- <img alt="Vue logo" src="./assets/logo.png" class="text-center"> -->
    <h1 class="text-center">{{ title }}</h1>

    <!-- <div class="container">
<div class="row">
  <div class="col">Task</div>
  <div class="col">Done</div>
</div>

  <div class="row" v-for="(task,index) in tasks" :key="index">
    <div class="col">{{ task.action }}</div>
    <div class="col">{{task.done}}</div>
  </div>
  </div> -->
    <div class="container">
      <div class="row">
        <h4>add item</h4>
        <div class="col">
          <input
            type="text"
            name=""
            class="form-control"
            v-model="newTask"
            v-on:keyup.enter="addTask"
          />
        </div>
        <div class="col">
          <input
            type="button"
            class="btn btn-primary"
            value="Create"
            v-on:click="addTask()"
          />
        </div>
      </div>

      <div class="mt-3" id="noData" v-if="filterTask.length == 0">
        <div
          class="alert alert-danger alert-dismissible fade show"
          role="alert"
        >
          <strong>No Data!</strong> You should add some task to Do.
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="alert"
            aria-label="Close"
          ></button>
        </div>
      </div>

      <div class="" id="dataHave" v-if="filterTask">
        <div class="fs-4 text-black-50 shadow-sm">
          Total FilterTasks - {{ filterTask.length }}
        </div>
        <table
          class="table table-white table-striped border border-white shadow-md"
        >
          <thead class="">
            <tr>
              <!-- <th scope="col">#</th> -->
              <th class="col-11 fs-3">Task</th>
              <th class="col fs-3">Done</th>
              <!-- <th scope="col">Handle</th> -->
            </tr>
          </thead>

          <!-- <tbody class="" v-for="(task,index) in tasks" :key="index">  -->

          <tbody class="" v-for="(task, index) in filterTask" :key="index">
            <tr>
              <!-- <th scope="row">1</th> -->
              <td class="" v-bind:class="task.done ? 'delete' : ''">
                {{ task.action }}
              </td>
              <td>
                <div class="form-check form-switch float-end me-5">
                  <input
                    class="form-check-input"
                    type="checkbox"
                    v-model="task.done"
                  />
                  <!-- <label class="form-check-label" for="flexSwitchCheckDefault">Default switch checkbox input</label> -->
                </div>
              </td>
              <!-- <td>@mdo</td> -->
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div class="btn btn-warning float-end me-3">
      Hide <input type="checkbox" v-model="hideCompleted" />
    </div>

    <input
      type="button"
      class="btn btn-danger float-end me-3"
      v-on:click="deleteTask()"
      value="Delete"
    />

    <!-- <button></button> -->
  </div>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    title: "todoList",
    hideCompleted: false,
    newTask: "",
    tasks: [
      // {
      //   action: " clean",
      //   done: true,
      // },
      // {
      //   action: " watch",
      //   done: false,
      // },
      // {
      //   action: " eat",
      //   done: true,
      // },
      // {
      //   action: " shit",
      //   done: false,
      // },
    ],
  }),

  computed: {
    filterTask() {
      return this.hideCompleted
        ? this.tasks.filter((filter) => !filter.done)
        : this.tasks;
    },
  },
  methods: {
    addTask() {
      // alert(this.newTask)
      // eslint-disable-next-line no-undef
      if (!this.newTask) {
        return alert("need to fill");
      }
      // alert(this.newTask);
      this.tasks.push({
        action: this.newTask,
        done: false,
      });

      this.storeData();
      this.newTask = "";
    },
    deleteTask() {
      // alert("delete");
      this.tasks = this.tasks.filter((d) => !d.done);
      this.storeData();
    },

    storeData() {
      localStorage.setItem("localStorageTasks", JSON.stringify(this.tasks));
    },
  },

  mounted() {
    let data = localStorage.getItem("localStorageTasks");
    // console.log(data);
    if (data) {
      this.tasks = JSON.parse(data);
    }
  },
};
</script>

<style>
.delete {
  text-decoration: line-through;
}
</style>
