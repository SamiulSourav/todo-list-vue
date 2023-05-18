<script setup>
    import AddTask from './components/add-task.vue';
    import ButtonComponent from './components/button-component.vue';
    import SimplifyComponent from './components/simplify-component.vue';
    import {computed, ref} from 'vue';
    let idcount = ref(0);
    const tasks = ref([])
    let state = ref(1);
    const init = ref('There is no task');
    const wrong = ref('');
    const empty = ref('');

    const filteredTask = computed(() => {
        if(state.value === 1) {
            return tasks.value;
        }
        else if(state.value === 2) {
            return tasks.value.filter((task) => task.check === true);
        }
        else if(state.value === 3) {
            return tasks.value.filter((task) => task.check === false);
        }
    })

    function handleInput(input){
        ++idcount.value;
        let exist = 0;
        for(let i=0;i<tasks.value.length;i++) {
            if(tasks.value[i].title === input) {
                exist = 1;
                break;
            }
        }
        if(exist === 1) {
            wrong.value = 'This task is already in the list!';
            setTimeout(() => {
                wrong.value = ''
            }, 1500)
        }
        else {
            wrong.value = '';
            if(input.length !== 0) {
                const obj = {id: idcount.value, check: false, title: input};
                tasks.value.push(obj);
                stateChecking();
            }
            else {
                empty.value = 'Task name cannot be empty';
                setTimeout(() => {
                    empty.value = ''
                }, 1500)
            }
        }
        console.log(tasks.value);
    }

    function updateState(flag){
        state.value = flag.value;
        stateChecking();
        console.log(state.value, tasks.value);
    }

    function clear(){
        tasks.value = tasks.value.filter((task) => task.check === false);
    }

    function deleteTask(deleteTaskId) {
        tasks.value = tasks.value.filter((task) => task.id !== deleteTaskId);
    }
    
    function stateChecking(){
        if(state.value === 1) {
            init.value = 'All tasks are here:';
        }
        else if(state.value === 2) {
             init.value = 'These tasks are already completed:';
        }
        else if(state.value === 3) {
            init.value = 'These tasks are still not completed:';
        }
        if(filteredTask.value.length === 0) {
            init.value = 'There is no task';
        }
    }


</script>

<template>
    <main>
        <div id = "container">
            <h1> Todo List </h1>
            <AddTask @take-input = "handleInput" /> 
            <br>
            <ButtonComponent @flag = "updateState" @clear-completed = "clear"/>
            <br> 
            {{ init }} <br>
            {{ wrong }}
            {{ empty }}
            <br> <br>
            <li id="li" v-for = "(task) in filteredTask" :key = "task.id" >
                <SimplifyComponent 
                    v-model:title = "task.title"
                    v-model:check = "task.check"
                    @delete-id = "deleteTask(task.id)"
                />
            </li>

            <!-- <br> <br> {{ tasks }} -->
        </div>
    </main>
    
</template>


<style>
    @import './style.css';
</style>
