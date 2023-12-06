<template>
  <div class="bg-lime-100 pb-20">
    <div
      class="min-[300px]:w-4/5 min-[900px]:w-2/3 sm:w-4/5 md:w-1/3 lg:w-1/2 xl:w-1/2 rounded-xl shadow-xl bg-slate-200 p-10 mx-auto"
    >
      <form
        @submit.prevent="addTask"
        class="bg-slate-100 rounded-md shadow-md w-full"
      >
        <div class="flex items-center border-b border-teal-500 py-2">
          <input
            required
            type="text"
            name="addTask"
            id="task"
            placeholder="New Task..."
            v-model="title"
            class="appearance-none bg-transparent border-none w-full text-gray-700 mr-3 py-2.5 px-4 leading-tight focus:outline-none"
          />
          <input
            type="time"
            name="startTime"
            id="startTime"
            v-model="startTime"
            class="px-6 py-2 lg:px-10 bg-gray-200 focus:outline-none rounded-full mr-4"
            required
          />
          <button
            type="submit"
            class="pr-6 text-sky-700 text-2xl flex-shrink-0 border-transparent py-1"
          >
            <fa icon="fas fa-solid fa-plus" />
          </button>
        </div>
      </form>

      <ul class="space-y-1 text-gray-800 my-10 w-full mx-auto text-center p-5">
        <li
          v-for="task in tasks"
          :key="task.id + ' ' + task.completed"
          class="border-b border-gray-300 py-2.5 flex justify-between w-full"
        >

        
           <button @click="isCompleted(task.id)">
            <p
              v-if="task.completed"
              class="w-full text-start text-md md:text-lg lg:text-xl"
            >
              <fa icon="fas fa-circle-xmark" class="text-red-800 mr-6" />
              <i class="font-mono line-through text-red-800"
                >{{ task.title }} {{ task.startTime }}</i
              >
            </p>
            <p
              v-else
              class="w-full text-start text-md md:text-lg lg:text-xl font-mono"
            >
              <fa icon="far fa-circle" class="text-sky-600 mr-4" />
              {{ task.title }} {{ task.startTime }}
            </p>
          </button>
        
          

          <div class="flex justify-between">
            <button @click="addPriority(task.id)" class="mx-auto">
              <p v-if="task.priority" class="text-yellow-600">
                <fa icon="fas fa-star" />
              </p>
              <p v-else>
                <fa icon="far fa-star" />
              </p>
            </button>

            <button @click="removeTask(task.id)" class="px-8">
              <fa icon="fas fa-trash" />
            </button>
          </div>
        </li>
      </ul>

      <div class="mt-5 flex">
        <button
          @click="clearCompleted"
          class="text-white bg-teal-600 hover:bg-teal-700 focus:outline-none focus:ring-4 focus:ring-teal-300 font-medium py-2.5 rounded-md text-sm md:text-md lg:text-lg text-center mr-2 mb-2 w-1/2"
        >
          Clear Completed
        </button>
        <button
          @click="clearTasks"
          class="text-white bg-teal-600 hover:bg-teal-700 focus:outline-none focus:ring-4 focus:ring-teal-300 font-medium rounded-md py-2.5 text-sm md:text-md lg:text-lg text-center mr-2 mb-2 w-1/2"
        >
          Clear All
        </button>
      </div>

      <p class="mt-5 font-bold md:text-lg lg:text-xl">
        <fa icon="fas fa-clipboard-list" class="mr-2 text-sky-800" />Pending
        Task : {{ incompeleteTasks }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoLogic",
  props: {
    tasks: Array,
  },

  data() {
    return {
      title: "",
      completed: true,
      startTime: "",
      priority: false,
    };
  },

  computed: {
    incompeleteTasks() {
      let taskCount = 0;

      for (let i = 0; i < this.tasks.length; i++) {
        if (this.tasks[i].completed === false) {
          taskCount++;
        }
      }
      return taskCount;
    },
  },

  methods: {
    clearTasks() {
      this.$emit("clear-all");
    },

    //  Insert a Single Task
    addTask() {
      console.log("emitted");
      this.$emit(
        "add-task",
        { title: this.title, completed: false, priority: false },
        this.startTime
      );
      this.title = "";
      this.startTime = "";
    },

    isCompleted(id) {
      this.$emit("is-completed", id);
    },

    removeTask(id) {
      this.$emit("remove-task", id);
    },

    clearCompleted() {
      this.$emit("clear-completed");
    },

    addPriority(id) {
      this.$emit("add-priority", id);
    },
  },
};
</script>

<style>
</style>
