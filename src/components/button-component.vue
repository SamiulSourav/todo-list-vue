<script setup>
    import { ref } from 'vue';
    let filteredTask = ref([]);
    let flag = ref('1');

    const emit = defineEmits(['flag','clear-completed'])

    const startText = ref('All tasks are here:');
    const cleartext = ref('');
    
    function showAll(){
        flag.value = 1;
        emit('flag',flag);
    }

    function completed(){
        flag.value = 2;
        emit('flag',flag);        
    }

    function notCompleted(){
        flag.value = 3;
        emit('flag',flag);
    }

    function clearCompleted(){
        // flag.value = 4;
        emit('clear-completed');
        cleartext.value = '... ... cleared completed ... ...';
        setTimeout(() => {
            cleartext.value = ''
        }, 1500)

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