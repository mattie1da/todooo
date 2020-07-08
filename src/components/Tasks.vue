<template>
  <div>
    <form @submit.prevent autocomplete="off">
      <ul v-if="tasks.length" class="tasks">
        <li class="task" v-for="(task, index) in tasks" :key="task.index" :class="{ 'task--complete' : task.complete }">
          <a href="#" class="task__icon task__icon--checkbox" @click.prevent="task.complete = !task.complete"></a>
          <input type="text" v-model="task.name" class="task__input" @keyup.enter="$event.target.blur()" :disabled="task.complete">
          <a href="#" @click.prevent="removeTask(index)" class="task__icon task__icon--remove">&times;</a>
        </li>
      </ul>

      <aside v-if="tasks.length" class="progress" :class="{ 'progress--active' : progress }">
        <span class="progress__completed" :style="{ width: progress + '%'}"></span>
      </aside>

      <div class="task task--create">
        <label for="new-task" class="task__icon task__icon--create">+</label>
        <input :placeholder="placeholder" id="new-task" type="text" v-model="newTask" @keyup.enter="addTask()" class="task__input">
        <!--<button class="task__submit" type="submit" @click.prevent="addTask(newTask)">Add task</button>-->
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Tasks',

    data() {
      return {
          tasks: [],
          editedTask: '',
          newTask: '',
      }
    },

    methods: {
        addTask() {
            if(this.newTask != '') {
                this.tasks.push({
                    name: this.newTask,
                    complete: false
                });
                this.newTask = '';
            }
        },

        removeTask(index) {
            this.tasks.splice(index, 1);
        }
     },

    computed: {
      completedTasks () {
          return this.tasks.filter(task => task.complete)
      },

      progress () {
          return this.completedTasks.length / this.tasks.length * 100;
      },

      placeholder () {
          if (!this.tasks.length) {
              return 'Create your first task';
          } else if (this.progress === 100) {
              return '🎉 All tasks done. See you tomorrow!';
          } else {
              return 'Create another task';
          }
      }
    },

    watch: {
        tasks: {
            handler() {
                localStorage.setItem('tasks', JSON.stringify(this.tasks));
            },
            deep: true
        }
    },

    mounted() {
        if (localStorage.getItem('tasks')) {
            this.tasks = JSON.parse(localStorage.getItem('tasks'));
        }
    },
}
</script>

<style scoped lang="scss">
  .tasks {
    padding-left: 25px;
    margin-bottom: 1rem;
  }

  .task {
    display: flex;
    padding: 0.5rem 0;
    position: relative;

    &:hover {
      .task__icon--remove,
      .task__icon--options {
        visibility: visible;
        opacity: 1;
      }
    }
    
    &--create {
      align-items: center;
      border-bottom: 1px dashed var(--grey);
      display: flex;
      margin-top: 2rem;
      padding: 0 0 .5rem 25px;
      position: relative;
    }

    &--complete {
      opacity: .4;

      .task__input {
        text-decoration: line-through;
      }

      .task__icon--checkbox {
        background: var(--dark);

        .app--dark & {
          background: var(--light);
        }
      }
    }
  }

  .task__input {
    background: var(--white);
    border: none;
    color: inherit;
    font-family: 'Poppins', sans-serif;
    font-size: 1rem;
    outline: none;
    width: 100%;
  }

  .task__icon {
    position: absolute;
    text-decoration: none;

    &--remove,
    &--options {
      color: lightgray;
      visibility: hidden;
      opacity: 0;
      padding: 5px;
    }

    &--remove {
      right: 1rem;
      top: 5px;

      &:hover {
        color: var(--error);
      }
    }

    &--options {
      right: 2.25rem;
      top: 4px;

      &:hover {
        color: var(--theme);
      }
    }

    &--checkbox {
      background: var(--light);
      border: 2px solid var(--dark);
      border-radius: 50%;
      left: -25px;
      width: .85rem;
      height: .85rem;

      top: 50%;
      transform: translateY(-50%);

      &:hover {
        background: var(--dark);
      }
     }

    &--create {
      color: var(--theme);
      font-size: 1.4rem;
      left: 2px;
      font-style: normal;
      margin-top: -4px;
      top: 0;
      transition: var(--transition);
      height: 15px;
      width: 15px;

      .app--black.app--dark & {
        color: var(--light);
      }
    }
  }

  .task__options {
    background: lightgray;
    border-radius: 1rem;
    display: flex;
    flex-direction: column;
    padding: .5rem;
    position: absolute;
    right: 2rem;
    z-index: 2;
  }

  .task__option {
    color: var(--dark);
    text-decoration: none;
  }

  .task__submit {
    display: none;
  }

  .error {
    color: var(--error);
  }

  .progress {
    border-radius: 2rem;
    height: 5px;
    overflow: hidden;
    position: relative;
    margin-top: 2rem;
    margin-bottom: 3rem;
    transition: var(--transition);

    &--active {
      background: rgba(lightgrey, 0.25);
    }
  }

  .progress__completed {
    background: var(--progress);
    bottom: 0;
    left: 0px;
    position: absolute;
    top: 0;
    transition: all 0.25s ease-in-out;
  }
</style>
