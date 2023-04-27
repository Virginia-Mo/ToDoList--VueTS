<template>
  <main>
    <div class="App">
      <h1>My To do list</h1>
        <InputField @taskFunction="addTask" />
        <TaskList :inProgress="inProgress" :toDo="toDo" @handleDelete="deleteTask" />
    </div>
  </main>
</template>

<script setup lang="ts">
import { reactive } from 'vue'
import InputField from './components/InputField.vue'
import TaskList from './components/TaskList.vue'
import type { taskModel } from "@/model/task";

let toDo : taskModel[] = reactive([]);
let inProgress : taskModel[] = reactive([]);
let doneTask : taskModel[] = []

function addTask(task : string){
 const newTask = {
    id : Date.now(),
    text : task,
    isDone : false,
    list: 1
 }
  toDo.push(newTask)
}

function deleteTask(id:number){
 let index = toDo.map(function(item) {
    return item.id
}).indexOf(id);
toDo.splice(index, 1);
console.log(toDo)
}
</script>




<style lang="scss">
.App {
    background: rgb(61,37,246);
    background: linear-gradient(171deg, rgba(61,37,246,0.9640231092436975) 2%, rgba(108,133,215,1) 49%, rgba(242,168,194,0.7763480392156863) 74%);
    background-image:url("@/assets/imgs/note.jpg") ;
    background-size: cover;
    font-family: 'Klee One', cursive;
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;

    & h1 {
        margin: 2rem;
        font-family: 'Permanent Marker', cursive;
        font-size: 7rem;
        font-weight: 600;
        color: rgba(243, 23, 23, 0.776);
    }
}
</style>
