<template>
    <div class="task-details">
        <h3 class="task-details__title">{{ task.title }}</h3>
        <div class="task-details__icons">
            <i 
                class="task-details__icon --del material-icons"
                @click="taskStore.deleteTask(task.id)"
            >delete</i>
            <i 
                class="task-details__icon --fav material-icons"
                :class="{'--active': task.isFav}"  
                @click="taskStore.toggleFav(task.id)"
            >favorite</i>
        </div>
    </div>
</template>

<script>
import { useTaskStore } from '../stores/TaskStore';

    export default {
        props: ['task'],
        setup() {
            const taskStore = useTaskStore();

            return { taskStore }
        }
    }
</script>

<style lang="scss">
    @import "../scss/extends";
    @import"../scss/mixins";

    .task-details {
        @extend %ex-card;
        display: flex;
        align-items: center;
        justify-content: space-between;
        &__title {
            color: var(--c-text-dark-1);
            font-size: 1.5em;
            line-height: 1;
        }
        &__icons {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0 20px;
        }
        &__icon {
            &.--del {
                @include mx-primary-icon;
            }
            &.--fav {
                @include mx-fav-icon;
            }
        }
    }
</style>