<script setup>
    import addTask from './components/add-task.vue';
    import buttonComponent from './components/button-component.vue';
    import {ref} from 'vue';
    let idcount = ref(0);
    const tasks = ref([])
    const inputText=ref('');


    let state = ref(1);
    const init = ref('"There is no task"');
    const wrong = ref('');
    const empty = ref('');

    const copyFilteredTask = ref([]);
    function handleInput(input){
        ++idcount.value;
        let exist = ref(0);
        for(let i=0;i<tasks.value.length;i++) {
            if(tasks.value[i].title === input) exist.value = 1;
        }
        if(exist.value === 1) {
            wrong.value = 'This task is already in the list!';
            setTimeout(() => {
                wrong.value = ''
            }, 1500)
        }
        else {
            wrong.value = '';
            if(input.length !== 0) {
                const obj=ref({id: idcount.value, check: false, title: input, edit: false});
                tasks.value.push(obj.value);
                stateChecking();
                empty.value = '';
                if(state.value === 1) init.value = '"All tasks are here:"';
                if(state.value === 3) init.value = '"These tasks are still not completed:"';
            }
            else {
                empty.value = 'Task name cannot be empty';
                setTimeout(() => {
                    empty.value = ''
                }, 1500)
            }
        }
    }

    function updateState(flag){
        state.value = flag.value;
        stateChecking();
    }

    function toggle(task){
        for(let i=0;i<copyFilteredTask.value.length;i++) {
            if(copyFilteredTask.value[i].id === task.id) {
                if (copyFilteredTask.value[i].check === true) copyFilteredTask.value[i].check = false;
                else copyFilteredTask.value[i].check = true;
            }
        }
        // tasks.value = copyFilteredTask.value;
        console.log(tasks);
        console.log(copyFilteredTask);
        stateChecking();
        console.log('aftercheck',copyFilteredTask.value);
        console.log('--',state.value);
    }

    function filter(filteredTask){
        console.log(state.value);
        copyFilteredTask.value = filteredTask.value;
        stateChecking();
    }

    function clear(filteredTask){
        tasks.value = filteredTask.value;
        if(state.value === 2) copyFilteredTask.value = [];
        else copyFilteredTask.value = tasks.value;
        console.log(tasks);
    }

    function deleteTask(delTask) {
        copyFilteredTask.value = tasks.value.filter(task => task.id !== delTask.id);
        tasks.value = copyFilteredTask.value;
        stateChecking();
        if(copyFilteredTask.value.length === 0) init.value = '"There is no task"'
    }

    function stateChecking(){
        if(state.value === 1) copyFilteredTask.value = tasks.value;
        else if(state.value === 2) copyFilteredTask.value = tasks.value.filter(task => task.check === true);
        else if(state.value === 3) copyFilteredTask.value = tasks.value.filter(task => task.check === false);
    }

    const inputVal = ref('');
    function editTask(task){
        inputVal.value = task.title;
        task.edit = true;
    }
    function editTaskDone(task){
        task.title = inputVal.value;
        task.edit=false;
    }

    function textChange(startText){
        init.value = startText;
        if(copyFilteredTask.value.length === 0) init.value = '"There is no task"';
    }

</script>

<template>
    <main>
        <div id="container">
            <h1> Todo List </h1>
            <addTask @take-input="handleInput" /> 
<br>
            <buttonComponent :tasks="tasks" @flag="updateState" @show-all="filter" 
            @completed="filter" @not-completed="filter" @clear-completed="clear" @text="textChange"/>
            <br> {{ init }} <br>
            {{ wrong }}
            {{ empty }}
            <br> <br>

            <li id="li" v-for="(task) in copyFilteredTask" :key="task.id">
                <input type="checkbox" class="checkbox" @click="toggle(task)" :checked="task.check">
                <label v-if="task.edit === false" @dblclick="editTask(task)" :class="{strikethrough: task.check}"> 
                    {{ task.title }}
                </label>
                <span v-if="task.edit">
                    <input style="padding-left: 5px; margin-left: 10px; margin-right: -45px;" autofocus class="inputtext" type="text" v-model="inputVal" @keyup.enter="editTaskDone(task)" @blur="editTaskDone(task)">
                </span>
                <button class="delete" style = "margin-left: 50px;" @click="deleteTask(task)">x</button>
            </li>

            <!-- <br> <br> {{ copyFilteredTask }} -->
        </div>
    </main>
    
</template>


<style>
    @import './style.css';
</style>
