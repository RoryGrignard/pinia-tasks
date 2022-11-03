<template>
  <main>
    <header class="header">
      <img class="header__img" src="./assets/pinia-logo.svg" alt="pinia logo">
      <h1 class="header__title">Pinia Tasks</h1>
    </header>

    <TaskForm />

    <nav class="filter">
      <button 
        class="filter__btn --all" 
        @click="filter = 'all'">
        All tasks
      </button>
      <button 
        class="filter__btn --favs" 
        :class="{ '--disabled': favCount === 0 }"
        @click="filter = 'favs'">
        Fav tasks
      </button>
    </nav>

    <div v-if="loading" class="loading">
      <h3 class="loading__title">Loading tasks...</h3>
    </div>

    <div v-if="filter === 'all'" class="task-list">
      <h2 class="task-list__title">All Tasks: <span class="task-list__title-count">{{ totalCount }}</span></h2>
      <div v-for="task in tasks" class="task-list__task" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

    <div v-if="filter === 'favs'" class="task-list">
      <h2 class="task-list__title">Favourite tasks: <span class="task-list__title-count">{{ favCount }}</span></h2>
      <div v-for="task in favs" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

  </main>
</template>

<script>
  import { ref } from 'vue';
  import { storeToRefs } from 'pinia';
  import { useTaskStore } from './stores/TaskStore';
  import TaskDetails from './components/TaskDetails.vue';
  import TaskForm from './components/TaskForm.vue';

  export default {
    components: { TaskDetails, TaskForm },
    setup () {
      const taskStore = useTaskStore();

      const { tasks, loading, favs, totalCount, favCount } = storeToRefs(taskStore);

      taskStore.getTasks();

      const filter = ref('all');

      return { taskStore, filter, tasks, loading, favs, totalCount, favCount };
    }
  }
</script>

<style lang="scss">
    @import "./scss/mixins";
    @import "./scss/extends";

  .header {
    color: var(--c-text-dark-1);
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0 40px;
    padding: 20px;
    &__img {
      max-width: 60px;
      transform: rotate(-10deg);
    }
    &__title {
      transform: rotate(2deg);
    }
  }

  .filter {
    @extend %ex-container;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0 20px;
    &__btn {
      @include mx-primary-btn;
      display: inline-block;
      border-radius: 6px;
      padding: 0 24px;
      line-height: 52px;
      font-size: 1.2rem;
      font-weight: 500;
      &.--disabled {
        opacity: .5;
        pointer-events: none;
      }
      &:hover {
        &.--all {
          transform: rotate(-3deg);
        }
        &.--favs {
          transform: rotate(3deg);
        }
      }
    }
  }

  .loading {
    &__title {
      color: var(--c--white)
    }
  }

  .task-list {
    @extend %ex-container;
    display: flex;
    flex-direction: column;
    gap: 20px 0;
    &__title {
      @include mx-section-title;
      display: flex;
      gap: 0 20px;
      align-items: center;
      justify-content: space-between;
      &-count {
        font-size: 1.5em;
      }
    }
  }
</style>