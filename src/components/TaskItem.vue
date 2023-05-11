<template>
  <form class="item todoItem" @submit.prevent="$emit('editText', props.item.id, inputText), edit = !edit">
    <input  v-if="edit"
            ref="inputElem"
            class='todoItem__input todoItem__p'
            type="text" 
            v-model="inputText"
            />
    <span v-if="props.item.isDone && !edit" class="todoItem__p todoItem__p--done">{{ props.item.text }}</span>
    <span v-if="!props.item.isDone && !edit" class="todoItem__p">{{ props.item.text }}</span>
    <div class="icons"> 
        <span class="icon"><Icon icon="material-symbols:edit" @click="isEditing"/></span>
        <span class="icon"><Icon icon="mdi:trash-can" @click="isDeleted"/></span>
        <span class="icon"><Icon icon="material-symbols:check" @click="isDone" /></span>
    </div>
  </form>
</template>

<script setup lang="ts">
import { nextTick, ref } from 'vue';
import type { taskModel } from '@/model/task';
import { Icon } from '@iconify/vue';
let inputText = ref("");

let edit = ref(false);
const inputElem = ref<HTMLInputElement>();
function isEditing(){
    edit.value = !edit.value;
    if (edit.value){
    nextTick(() => {
        inputElem.value?.focus();
    })}
}

const props = defineProps<{
  item: taskModel
}>()

const emit = defineEmits<{
  (e: 'doneF', id:number): void,
  (e: 'deleteF', id:number): void,
}>()
function isDone(){
    emit("doneF", (props.item.id))
}
function isDeleted(){
    emit("deleteF", (props.item.id))
}
</script>

<style scoped lang="scss">
.todoItem {
    display: flex;
    width: 80%;
    border-radius: 0.5rem;
    padding: 0.5rem;
    margin: 0.5rem auto;
    gap: 4rem;
    background-image:url("@/assets/imgs/note.jpg") ;
    background-size: cover;
    transition: 0.2s;

    &:hover {
       box-shadow: 0 0 0.5rem black;
       transform: scale(1.03);
    }
    &__p {
        flex: 1;
        padding: 0.5rem;
        margin-left: 4rem;
        &:focus {
            outline: none;
        }
        &--done {
            text-decoration: line-through;
        }
    }
    & div {
        display: flex;
        align-items: center;
    }
}
.icon {
    margin-left: 1rem;
    font-size: 2.5rem;
    cursor: pointer;
}
.icons {
    display: flex;
    flex-wrap: wrap;
}

@media screen and (max-width: 768px) {
    .todoItem {
        width: 90%;
        gap: 0.5rem;
       word-break: break-all;
    }
}
</style>
