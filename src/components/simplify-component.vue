<script setup>
    import { nextTick, ref, watch } from 'vue';
    const props = defineProps({
        title:{
            type: String,
            required: true
        },
        check:{
            type: Boolean,
            required: true
        }
    })

    const emit = defineEmits(['delete-id', 'update:title', 'update:check'])

    const edit = ref(false);
    const title = ref('');
    title.value = props.title;
    const check = ref(false);
    check.value = props.check;

    function toggle() {
        if(check.value === true) check.value = false;
        else check.value = true;
        emit('update:check',check);
    }

    let inputVal = ref('');
    let editReady = 0;

    function editTask() {
        edit.value = true;
        inputVal.value = title.value;
    }

    function editTaskDone() {
        title.value = inputVal.value;
        edit.value = false;
        emit('update:title',title);
    }

    function deleteTask(){
        emit('delete-id');
    }

    const inputField = ref(null);
    watch (() => edit.value,(n) => {
        if (n === true) {
            nextTick(() => {
                inputField.value.focus();
            })
        }
    }) 

</script>

<template>

    <input type = "checkbox" class = "checkbox" @click = "toggle" :checked="check" @update:check="check">

    <label v-if="edit === false" style = "padding-right: 50px;" @dblclick="editTask" :class="{strikethrough: check}">
        {{ title }}
    </label>
    <input 
        v-if = "edit === true" ref = "inputField" 
        style = "padding-left: 5px; margin-right: -45px;" 
        class = "inputtext" v-model = "inputVal" 
        @keyup.enter = "editTaskDone" @blur = "editTaskDone"
    >
    <button class = "delete" @click = "deleteTask">x</button>

</template>
