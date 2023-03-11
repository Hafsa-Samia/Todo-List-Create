<template>
  <div>
    <div class="flex bg-lime-100 m-auto text-start">
      <img alt="Vue logo" src="./assets/logo.png" class="ml-auto" />
      <h1
        class="text-2xl lg:text-5xl md:text-4xl sm:text-3xl font-extrabold my-auto mr-auto"
      >
        Create a ToDo List
      </h1>
    </div>
    <TodoLogic
      v-bind:tasks="tasks"
      v-on:clear-all="clearAllClickedFromChild"
      v-on:add-task="addNewTask"
      v-on:is-completed="completedTask"
      v-on:remove-task="deletedTask"
      v-on:clear-completed="removeAllCompletedTasks"
    />
  </div>
</template>

<script>
import TodoLogic from "./components/TodoLogic.vue";

export default {
  name: "App",
  components: {
    TodoLogic,
  },

  data() {
    return {
      tasks: [
        {
          id: 1,
          title: "Homework",
          completed: false,
          startTime: new Date().toLocaleTimeString("en-US", {
            timeZone: "Asia/Dhaka",
            hour12: true,
            hour: "numeric",
            minute: "numeric",
          }),
        },
        {
          id: 2,
          title: "Project",
          completed: false,
          startTime: new Date().toLocaleTimeString("en-US", {
            timeZone: "Asia/Dhaka",
            hour12: true,
            hour: "numeric",
            minute: "numeric",
          }),
        },
        {
          id: 3,
          title: "Reading",
          completed: false,
          startTime: new Date().toLocaleTimeString("en-US", {
            timeZone: "Asia/Dhaka",
            hour12: true,
            hour: "numeric",
            minute: "numeric",
          }),
        },
      ],
    };
  },

  methods: {
 //insert a New Task into tasks Array
    addNewTask(newTask, rawTime) {
      newTask.id = this.tasks.length + 1;
      let parsableTime = "2023-01-01T" + rawTime + "+0600";      
      let timeObject
      try {
        timeObject = new Date(parsableTime);
      } catch {
        timeObject = null
      }

      if (timeObject == null) {
        newTask.startTime = "10:00 PM";
      } else {
        newTask.startTime = timeObject.toLocaleTimeString("en-US", {
          timeZone: "Asia/Dhaka",
          hour: "2-digit",
          minute: "2-digit",
          hour12: true,
        });
      }
      this.tasks.push(newTask);
    },


  //Mark a task as completed
    completedTask(id) {
      let newTasksList = [];
      newTasksList = this.tasks;
      //console.log(newTasksList);

      for (var i = 0; i < newTasksList.length; i++) {
        if (newTasksList[i].id === id) {
          newTasksList[i].completed = !newTasksList[i].completed;
        }
      }
    },

    clearAllClickedFromChild() {
      this.tasks = [];
    },

   
    deletedTask(id) {
      let newTasksList = [];
      newTasksList = this.tasks;

      for (var i = 0; i < newTasksList.length; i++) {
        if (newTasksList[i].id == id) {
          newTasksList.splice(i, 1);
        }
      }

      this.tasks = newTasksList;

      //console.log('deleted',newTasksList);
    },


    removeAllCompletedTasks() {
      let newTasksList = [];
      newTasksList = this.tasks;
      for (var i = 0; i < newTasksList.length; i++) {
        if (newTasksList[i].completed === true) {
          newTasksList.splice(i, 1);
          i--;
        }

        console.log("task", newTasksList);
      }

      this.tasks = newTasksList;
    },
  },
};
</script>

<style>
</style>
