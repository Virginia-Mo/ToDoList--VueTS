
<template>
  <div 
  class='container'>
    <div
      class='todos'
      @drop="onDrop($event, 1)"
      @dragenter.prevent
      @dragover.prevent>
         <span class="todos__headings">
          Tasks to do
        </span>
    <div
    v-for="item in getList(1)"
    :key="item.id"
    draggable="true"
    @dragstart="startDrag($event,item)">
       <TaskItem
            :item="item"
            @doneF="isDone"
            @deleteF="isDeleted"
            @editText="isEdited"
            /> 
    </div>
 </div>    
 <div
      class='todos progress'
      @drop="onDrop($event, 2)"
      @dragenter.prevent
      @dragover.prevent>
         <span class="todos__headings">
          Tasks in Progress
        </span>
    <div v-for="item in getList(2)"
    :key="item.id"
    draggable="true"
    @dragstart="startDrag($event,item)">
       <TaskItem
            :item="item"
            @doneF="isDone"
            @deleteF="isDeleted"
            @editText="isEdited"
            /> 
    </div>
 </div> 
 <div
      class='todos remove'
      @drop="onDrop($event, 3)"
      @dragenter.prevent
      @dragover.prevent>
         <span class="todos__headings">
          Tasks Completed
        </span>
    <div v-for="item in getList(3)"
    :key="item.id"
    draggable="true"
    @dragstart="startDrag($event,item)">
       <TaskItem
            :item="item"
            @doneF="isDone"
            @deleteF="isDeleted"
            @editText="isEdited"
            /> 
    </div>
 </div> 
  
       </div>
</template>

<script setup lang="ts">
import TaskItem from './TaskItem.vue';
import type { taskModel } from '@/model/task';

const getList = (list : number) =>{
  return props.toDo.filter((item) => item.list === list)
}

const startDrag = (event: DragEvent, item: taskModel) => {
  if (!event.dataTransfer) {
    return;
  }
  event.dataTransfer.dropEffect = "move";
  event.dataTransfer.effectAllowed = "move";
  event.dataTransfer.setData("itemID", item.id)
}
const onDrop = (event: DragEvent, list: number) => {
  if (!event.dataTransfer) {
    return;
  }
  const itemID = event.dataTransfer.getData("itemID");
  const foundItem = props.toDo.find((item) => item.id === itemID)
  if (foundItem){ 
    foundItem.list = list
      if (list === 3){
        foundItem.isDone = true
      } else {
        foundItem.isDone = false
      }
  }
 event.target.classList.add("drop")
 setTimeout(() => {
  event.target.classList.remove("drop")
 }, 200);
}
const props = defineProps<{
  toDo: taskModel[],
  inProgress: taskModel[],
}>()

function isDone(id:number){
  const foundItem = props.toDo.find((item) => item.id === id)
  if (foundItem){
    foundItem.isDone = !foundItem.isDone
  }
}

function isDeleted(id:number){
const foundItem = props.toDo.find((item) => item.id === id)
  if (foundItem){
  emit("handleDelete", (foundItem.id))}
}
const emit = defineEmits<{
  (e: 'handleDelete', id:number): void,  
}>()

function isEdited(id:number, text:string){
    if (text === ""){
        return}
  const foundItem = props.toDo.find((item) => item.id === id)
  if (foundItem){
    foundItem.text = text
  }
}
</script>


<style lang="scss">
@use 'sass:color';
.drop {
  animation: tilt-shaking 0.3s ease-in-out;
}
@keyframes tilt-shaking {
  0% { transform: rotate(0deg); }
  25% { transform: rotate(5deg); }
  50% { transform: rotate(0eg); }
  75% { transform: rotate(-5deg); }
  100% { transform: rotate(0deg); }
}

.todos{
    display: flex;
    flex-direction: column;
    width: 50%;
    background-color: #40a3ff;
    border-radius: 0.5rem;
    padding-bottom: 1rem;
    font-size: 2rem;
    
    &__headings {
      text-align: center;
      padding: 1rem 0;
      font-weight: 600;
      font-size: 2.5rem;
    }
  }
  .container {
    display: flex;
    width: 95%;
    justify-content: space-evenly;
    margin-top: 1rem;
    gap: 1rem;
}
.remove {
    background-color: rgb(138 249 129);
}
.progress {
    background-color:#FFC107;
}
@media screen and (max-width: 768px) {
    .container {
        flex-direction: column;
    }
    .todos {
        width: 95%;
    }
}</style>