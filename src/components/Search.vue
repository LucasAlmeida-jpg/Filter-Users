<template>
  <form @submit.prevent="searching">
    <InputText 
      v-model="searched"
      placeholder="Search..."
      class="p-inputtext-lg"
      @keyup="highlighting"
    />

    <Button
      label="Search"
      type="submit"
      icon="pi pi-search"
      class="p-button-secondary p-button-lg button-search"
    />
  </form>
</template>

<script setup>
import InputText from 'primevue/inputtext';
import Button from 'primevue/button';
import {ref} from 'vue';

const searched = ref('');

const emit = defineEmits(['searching-user']);

function highlighting(){
  if(event.keyCode !== 13 && event.type !== 'submit'){
    emit('searching-user', {type:'highlight',searching:searched.value});
  }
}

function searching(event){
  if(event.keyCode === 13 || event.type === 'submit'){
    emit('searching-user', {type:'search',searching:searched.value});
  }
}

</script>

<style scoped>
.button-search {
  margin-left: 5px;
}
</style>