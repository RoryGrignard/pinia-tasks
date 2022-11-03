<template>
    <div class="task-form">
        <h2 class="task-form__title">Add new task:</h2>
        <form class="task-form__form" @submit.prevent="handleSubmit">
            <input 
                class="task-form__input"
                type="text" 
                placeholder="I need to..."
                v-model="newTask"
            >
            <button class="task-form__btn">
                <span class="task-form__icon material-icons-outlined">add_circle</span>
            </button>
        </form>
    </div>
</template>

<script>
import { ref } from 'vue';
import { useTaskStore } from '../stores/TaskStore';

    export default {
        setup() {
            const taskStore = useTaskStore();

            const newTask = ref();

            const handleSubmit = () => {
                if (newTask.value.length > 0) {
                    taskStore.addTask({
                        title: newTask.value,
                        isFav: false,
                        id: Math.floor(Math.random() * 10000)
                    });
                    newTask.value = '';
                }
            }

            return { handleSubmit, newTask }
        }
    }
</script>

<style lang="scss" scoped>
    @import "../scss/mixins";
    @import "../scss/extends";
    .task-form {
        @extend %ex-container;
        @extend %ex-card;
        display: flex;
        flex-direction: column;
        gap: 20px 0;
        &__title {
            @include mx-section-title;
        }
        &__form {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 0 10px;
        }
        &__input {
            outline: none;
            padding: 10px 20px;
            font-size: 1.35rem;
            line-height: 1;
            flex: 1;
            border: 2px solid var(--c-brand);
            border-radius: 6px;
            color: var(--c-text-dark-1);
            background-color: transparent;
            transition: border-color .1s ease;
            &:hover {
                border-color: var(--c-brand);
            }
            &:focus {
                border-color: var(--c-brand-light);
            }
            &::placeholder {
                color: var(--c-text-light-2);
            }
        }
        &__btn {
            background-color: transparent;
            padding: 0;
            margin: 0;
            width: 46px;
            height: 46px;
            border-radius: 50%;
            overflow: hidden;
        }
        &__icon {
            @include mx-primary-icon;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 100%;
            height: 100%;
            font-size: 24px;
        }
    }
</style>