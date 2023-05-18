<script setup>
    import { ref } from 'vue';
    // import tasks from '../App.vue';

    const emit = defineEmits(['take-input'])
    const input = ref('');
    function addtask(){
        input.value='';
    }
</script>

<template>
    <input autofocus class="inputtext" v-model="input" 
    @keydown.enter="emit('take-input', input)" @keyup.enter="addtask" placeholder="enter the task to be done" />

    <button class="add" @mousedown="$emit('take-input',input)" @mouseup="addtask">
        Add
    </button>
    <!-- {{ input }} -->
</template>