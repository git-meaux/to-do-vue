<script setup>

</script>

<template>
    <button>
        <slot>Create</slot>
    </button>
</template>

<style lang="scss" scoped>
button {
    padding: 8px 16px;
    border: none;
    border-top-right-radius: 2rem;
    border-bottom-right-radius: 2rem;
    &:hover {
        cursor: pointer;
    }
}
</style>