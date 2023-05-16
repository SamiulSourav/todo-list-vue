<script setup>
    import { ref } from 'vue';
    let filteredTask = ref([]);
    let flag = ref('1');

    const props = defineProps({
        tasks:{
            type: Array,
            required: true
        }
    })

    const emit = defineEmits(['show-all','completed','not-completed','clear-completed','flag','text'])

    const startText = ref('All tasks are here:');
    const cleartext = ref('');
    
    function showAll(){
        console.log(props)
        flag.value = 1;
        filteredTask.value = props.tasks;
        emit('show-all',filteredTask);
        emit('flag',flag);
        displayText();
        cleartext.value = '';
    }

    function completed(){
        flag.value = 2;
        filteredTask.value = props.tasks.filter(task => task.check === true);
        emit('completed',filteredTask);
        emit('flag',flag);
        displayText();
        cleartext.value = '';        
    }

    function notCompleted(){
        flag.value = 3;
        filteredTask.value = props.tasks.filter(task => task.check === false);
        emit('not-completed',filteredTask);
        emit('flag',flag);
        displayText();
        cleartext.value = '';
    }

    function clearCompleted(){
        filteredTask.value = props.tasks.filter(task => task.check === false);
        emit('clear-completed',filteredTask);
        console.log(filteredTask.value.length);
        let dif = props.tasks.length - filteredTask.value.length;
        if (dif === 0) cleartext.value = '... No task to clear. All tasks are incomplete! ...'
        else cleartext.value = '... ... cleared completed tasks ... ...';
        setTimeout(() => {
            cleartext.value = ''
        }, 1500)
        displayText();
    }

    function displayText(){
        if(filteredTask.value.length === 0) {
            startText.value = 'there is no tasks';
        }
        else{
            if(flag.value === 1) startText.value = 'All tasks are here:';
            if(flag.value === 2) startText.value = 'The completed tasks are here:';
            if(flag.value === 3) startText.value = 'These tasks are still not completed:';
        }
        emit('text',startText);
    }

    
</script>

<template>
<br> <br>
    <button class="showAllBtn" @click="showAll" :class="{showAllBtnClick: (flag===1)}">show all</button>
    <button class="completedBtn" @click="completed" :class="{completedBtnClick: (flag===2)}">completed</button>
    <button class="notCompletedBtn" @click="notCompleted" :class="{notCompletedBtnClick: (flag===3)}">not completed</button>
    <button class="clearBtn" @click="clearCompleted">clear completed</button>
    <br> <br>
    {{ cleartext }}
</template>