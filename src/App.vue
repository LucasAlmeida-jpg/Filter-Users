<template>
  <h2>My App</h2>

  <Search @searching-user="searching" />

  <div class="chip">
    <Chip :label="highlightCount.toString()" icon="pi pi-user" class="chipHighlight" />
  </div>

  <ul>
    <li v-for="user in users['data']" :key="user.id"
      :class="{ highlight: highlight.length > 0 && user.firstName.toLowerCase().includes(highlight.toLowerCase()) }">
      {{ user.firstName }} {{ user.lastName }}
    </li>
  </ul>
</template>

<script setup>
import { reactive, onMounted, ref } from 'vue';
import Search from './components/Search.vue';
import fakeData from './assets/json/MOCK_DATA.json';
import Chip from 'primevue/chip';

const users = reactive({ data: [] });
const highlight = ref('');
const highlightCount = ref(0);

onMounted(() => {
  users['data'] = fakeData;
})


function filterUsers(searched) {
  return users['data'].filter(user => user.firstName.toLowerCase().includes(searched.searching.toLowerCase()));
}

function searching(searched) {

  if (!searched.searching.length) {
    highlight.value = '';
    users['data'] = fakeData;
    highlightCount.value = 0;
    return;
  }

  if (searched.type === 'search') {
    users['data'] = filterUsers(searched);
  } else {
    highlight.value = searched.searching;
    highlightCount.value = filterUsers(searched).length;
  }

}

</script>

<style scoped>
ul {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

ul li {
  background-color: beige;
  color: black;
  padding: 15px;
  margin-right: 4px;
  margin-bottom: 5px;
  border-radius: 32px;
  border: 1px solid burlywood;
  font-weight: bold;
}

.highlight {
  background-color: blueviolet;
  color: white;
  border: 1px solid white;

}
.chip{
  margin-top: 20px;
}
</style>