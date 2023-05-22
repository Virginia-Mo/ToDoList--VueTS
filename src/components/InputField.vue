<template>
  <form  action="" class="input" @submit.prevent="addNewTask" >
        <input 
        type="text" 
        ref="formRef"
        class="input__box"
        name="todo_text"
        v-model="task"
        placeholder="Enter your task"
         />
        <button type="submit" class="input__button">GO</button>
       </form> 
</template>


<script setup lang="ts">
import { ref } from 'vue';
let task: string = ""
const formRef = ref<HTMLInputElement>()

const emit = defineEmits<{
  (e: 'taskFunction', task:string): void
}>()
function addNewTask(){
    emit("taskFunction",(task))
    if (formRef.value !== undefined){
        formRef.value.blur()
        formRef.value.value = ""
    }
}

</script>

<style scoped lang="scss">
@use '@/assets/styles/vars' as v;

.input {
    display: flex;
    width: 90%;
    position: relative;
    align-items: center;
    
    &__box {
        width: 60%;
        margin: auto;
        border-radius: 5rem;
        padding: 2rem 3rem;
        font-size: 2.5rem;
        border: none;
        font-family: 'Klee One', cursive;
        box-shadow: inset 0 0 0.5rem black;
        transition: 0.2s;

        &:focus {
            outline: none;
            box-shadow: 0 0 1rem 100rem  v.$color-dark;
        }
    }
    &__button {
        position: absolute;
        width: 5.5rem;
        height: 5.5rem;
        margin: 1.8rem;
        border-radius: 50px;
        right: 19.5%;
        border: none;
        font-size: 1.5rem;
        background-color: v.$color-dark;
        color: #fff;
        transition: 0.2s all;
        box-shadow: 0 0 1rem black;

        &:hover {
            background-color: v.$color-light;
        }
        
        &:active {
            transform: scale(0.8);
            box-shadow: 0 0 0.5rem black;
        }
    }
}
@media screen and (max-width: 768px) {
    .input {
        &__box {
            width: 80%;
        }
        &__button {
            right: 5%;
        }
    }
}
</style>
