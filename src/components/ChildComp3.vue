<template>
    <h2>Child component</h2>
</template>

<script setup lang="ts">
    const emit = defineEmits(['response'])

    emit('response', 'hello from child')
</script>
