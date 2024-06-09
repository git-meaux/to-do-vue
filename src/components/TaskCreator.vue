<script setup>
import { defineEmits, reactive } from 'vue';
import TaskButton from './TaskButton.vue';

const emit = defineEmits(['create-task']);

const taskState = reactive({
    task: '',
    invalid: null,
    errormessage: '',
});

const creatTask = () => {
    taskState.invalid = null;
    if (taskState.task !== '') {
        emit('create-task', taskState.task);
        taskState.task = '';
        return;
    }
    taskState.invalid = true;
    taskState.errormessage = 'Task cannot be empty';
};
</script>

<template>
    <div class="input-wrap" :class="{ 'input-err': taskState.invalid }">
        <input v-model="taskState.task" type="text" placeholder="Write task" />
        <TaskButton @click="creatTask()"></TaskButton>
    </div>
    <p v-if="taskState.invalid" class="err-msg">{{ taskState.errormessage }}</p>
    <!-- <p class="err-msg">{{ taskState.errormessage }}</p> -->
</template>

<style lang="scss" scoped>
.input-wrap {
    display: flex;
    transition: 250ms ease;
    border: 2px solid #41b080;
    border-radius: 2rem;

    &:focus-within {
        box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1),
            0 -2px 4px -2px rgb(0 0 0 / 0.1);
    }

    &.input-err {
        border-color: red;
    }

    input {
        width: 100%;
        padding: 8px 16px;
        border: none;
        border-radius: 2rem;
        // border-top-left-radius: 2rem;
        // border-bottom-left-radius: 2rem;

        &:focus {
            outline: none;
        }

    }

    // button {
    //     padding: 8px 16px;
    //     border: none;
    //     border-top-right-radius: 2rem;
    //     border-bottom-right-radius: 2rem;
    // }

}

.err-msg {
    margin-top: 6px;
    font-size: 12px;
    text-align: center;
    color: red;
}
</style>