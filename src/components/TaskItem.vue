<script setup>
    import { Icon } from '@iconify/vue';
    const props = defineProps({
        task: {
            type: Object,
            required: true,
        },
        index: {
            type: Number,
            required: true,
        },
    });
    defineEmits(['toggle-complete', 'edit-task', 'update-task', 'delete-task']);
</script>

<template>
    <div>
        <li>
            <input 
            type="checkbox"
            :checked="task.isCompleted" 
            @input="$emit('toggle-complete', index)"
            />
            <div class="task">
                <input v-if="task.isEditing" 
                type="text" :value="task.task" 
                @input="$emit('update-task', $event.target.value, index)"
                />
                <span v-else :class="{'completed-task' : task.isCompleted}">{{ task.task }}</span>
            </div>
            <div class="task-actions">
                <Icon v-if="task.isEditing" class="icon" 
                icon="ph:check-circle-bold" 
                width="22" height="22"  style="color: #26a269"
                @click="$emit('edit-task', index)"/>
                <Icon v-else class="icon" 
                icon="ph:pencil-simple-bold" 
                width="22" height="22"  style="color: #26a269"
                @click="$emit('edit-task', index)"
                />
                <Icon class="icon" icon="ph:trash-simple-bold" width="22" height="22"  style="color: #f95e5e"
                @click="$emit('delete-task', task.id)"/>
            </div>
        </li>
    </div>
</template>

<style lang="scss" scoped>
li {
  display: flex;
  align-items: center;
  gap: 10px;
  border-radius: 4px;
  padding: 16px 10px;
  outline: #8fcfbc 2px solid;
  background-color: #e9f1ef;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);
  &:has(.completed-task) {
    background-color: white;
  }
  &:hover {
    .task-actions {
      opacity: 1;
    }
  }

  input[type="checkbox"] {
    appearance: none;
    width: 20px;
    height: 20px;
    background-color: #fff;
    border-radius: 50%;
    cursor: pointer;

    &:checked {
      background-color: #41b080;
    }
  }

  .task {
    flex: 1;

    .completed-task {
        text-decoration-line: line-through;
        text-decoration-thickness: 2px;
        color: grey;
    }

    input[type="text"] {
      width: 100%;
      padding: 1px 6px;
      border: 2px solid #41b080;
    }
  }

  .task-actions {
    display: flex;
    gap: 6px;
    opacity: 0;
    transition: 150ms ease-in-out;
    .icon {
      cursor: pointer;
    }
  }
}
</style>