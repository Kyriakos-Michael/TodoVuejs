<template>
<div class="border: 0">

<div class="col mb-4">
      <button type="button" class="btn btn-primary"  
      id="mark-all" @click="selectAll"><i class="fas fa-arrow-down"></i></button>
    </div>

      <div class="card shadow-lg" id="todo">
          <div class="card-header text-left p-0">
            <input class="text-input w-100" v-model="newTask" @keyup.enter="addTask" placeholder="What needs to be done?" v-autofocus>
        </div>

          <div class="card-body p-0 text-left">
            <ul class="list" v-if="showAll">

                <li class="list-item" v-for="(task, key,index) in tasks" :key="index" :class="{ done : isChecked(task) }">

                    <input type="checkbox" class="checkbox" @click="check" v-model="task.checked">

                    <input class="text-input pl-3" type="text" 
                      v-if="task === editingTask" v-auto-focus  
                      @keyup.enter="endEditing(task)" @blur="endEditing(task)"
                     v-model="task.text">

                    <!-- Label for Showing each task in Tasks Array -->
                    <label for="checkbox" v-if="task !== editingTask && filters !== 'All' && filters !== 'Active' && filters !== 'Completed' " @dblclick="editTask(task)">
                      {{ task.text }}
                    </label>

                    <label for="checkbox" v-if="task !== editingTask && filters === 'All'"  @dblclick="editTask(task)">
                      {{ task.text }}           
                      </label>

                    <label for="checkbox" v-if="task !== editingTask && filters === 'Active' && task.checked !== true"  @dblclick="editTask(task)">
                     {{ task.text }}
                      </label>

                    <label for="checkbox" v-if="task !== editingTask && filters === 'Completed' && task.checked !== false"  @dblclick=" editTask(task)">
                      {{ task.text }}
           
                    </label>

                    <!-- Remove Tasks from Tasks Array -->
                    <button type="button" class="close delete" aria-label="Close" @click="removeTask(task)">
                    <span aria-hidden="true" style="font-size: 2rem;color: pink">&times;</span>
                    </button>

                </li>
            </ul>
          </div>

            <div class="card-footer bg-white">
              <div class="row">
                <div class="col">
                  <button class="btn btn-link" disabled>
                  {{ tasks.length === 1 ? '1 Item Left' :   tasks.length + ' Items Left'}}
                  </button>               
                </div>

                <div class="col">
                  <!-- Show all Todos -->
                   <button class="btn btn-link" @click="newValueAll">All</button>
                   <!-- Only Actives and not marked as completed -->
                    <button class="btn btn-link" @click="newValueActive">Active</button>
                    <!-- All Selected -->
                     <button class="btn btn-link" @click="newValueCompleted">Completed</button>
                </div>

                <!-- Clear Completed Tasks -->
                <div class="col">
                    <button class="btn btn-link"
                      v-if="areAllSelected === true" @click="clearList">
                      Clear Completed</button>
                </div>
               
              </div>
  
            </div>  
        </div>      
</div>
</template>

<script>
export default {
  data() {
    return {
      showAll: true,
      filters: "",
      newTask: "",
      tasks: [
        {
          text: "My First Todo",
          checked: false
        }
      ],
      editingTask: {}
    };
  },
  computed: {
    areAllSelected() {
      return (
        this.tasks.every(function(task) {
          return task.checked;
        }) && this.tasks.length > 0
      );
    }
  },
  methods: {
    addTask() {
      let task = this.newTask.trim();
      if (task) {
        this.tasks.push({ text: task, checked: false });
        this.newTask = "";
      }
    },
    newValueAll() {
      this.filters = "All";
    },
    newValueActive() {
      this.filters = "Active";
    },
    newValueCompleted() {
      this.filters = "Completed";
    },
    removeTask(task) {
      let index = this.tasks.indexOf(task);
      this.tasks.splice(index, 1);
    },

    editTask(task) {
      this.editingTask = task;
    },

    endEditing(task) {
      this.editingTask = {};
      if (task.text.trim() === "") {
        this.removeTask(task);
      }
    },

    clearList(task) {
      let index = this.tasks.indexOf(task);
      this.tasks.splice(index.length);
    },
    selectAll(task) {
      let targetValue = this.areAllSelected ? false : true;
      for (let i = 0; i < this.tasks.length; i++) {
        this.tasks[i].checked = targetValue;
      }
    },

    check(task) {
      task.checked = true;
    },

    isChecked(task) {
      return task.checked;
    }
  },
  directive: ("auto-focus",
  {
    bind: function() {
      Vue.nextTick(
        function() {
          this.el.focus();
        }.bind(this)
      );
    }
  })
};
</script>

<style scoped>
ul,
li {
  margin: 0;
  padding: 0;
  border: 0;
}

body {
  line-height: 1;
  font-family: "Lato", sans-serif;
}

.container {
  margin: 1em auto 3em;
  border: 1px solid #efefef;
}

.panel,
li {
  display: flex;
  list-style-type: none;
  padding: 10px;
  border-bottom: 1px solid #efefef;
  background-color: #e7e8eb;
}

.text-input {
  height: 4rem;
  color: black;
  font-size: 4rem;
}

.list li {
  background-color: white;
  color: black;
}

.list li button {
  background-color: transparent;
  border: 1px solid #3465a4;
  color: #ddd;
  visibility: hidden;
  font-size: 20px;
  font-weight: bold;
}

.list li:hover > button {
  visibility: visible;
}

.list label {
  padding-right: 10px;
  display: inline-block;
  width: 70%;
  font-size: 18px;
  line-height: 24px;
  color: black;
  z-index: 2;
  overflow: hidden;
}

.list li.done label {
  color: black;
  text-decoration: line-through;
}

.credit {
  margin: 1em auto 5em;
  text-align: center;
  font-size: 13px;
  line-height: 0.6;
  color: #999;
}

.credit a {
  text-decoration: none;
  color: #999;
}

.credit a:hover {
  text-decoration: underline;
}

.hidden {
  display: none;
}

.text-input,
.list label {
  width: 100%;
  font-size: 1rem;
}
.delete {
  text-decoration: none;
  border: 0 !important;
  font-size: 2.1rem;
  color: black;
}
.btn-link {
  color: black !important;
}
</style>
