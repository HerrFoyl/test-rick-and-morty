<template>
  <div>
    <h1>Rick and Morty Characters</h1>
    <div class="filters">
      <label for="name">Name:</label>
      <input v-model="filters.name" id="name" type="text" />

      <label for="status">Status:</label>
      <select v-model="filters.status" id="status">
        <option value="">All</option>
        <option value="Alive">Alive</option>
        <option value="Dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>

      <button @click="applyFilters">Apply</button>
    </div>
    <div v-if="loading" class="loading">Loading, please wait</div>
    <div v-else class="character-list">
      <div v-for="character in characters" :key="character.id">
        <CharacterCard :character="character" />
      </div>
      <div class="pagination">
        <button :disabled="!pagination.prev" @click="fetchCharacters(pagination.prev)">Previous</button>
        <button :disabled="!pagination.next" @click="fetchCharacters(pagination.next)">Next</button>
    
      </div>
    </div>
  </div>
</template>

<script setup>
  import {ref, onMounted} from 'vue'
  import CharacterCard from './components/CharacterCard.vue'

  const characters = ref([])
  const loading = ref(false)
  const filters = ref({name: '', status: ''})
  const pagination = ref({next: null, prev: null})

  const fetchCharacters = async (url = 'https://rickandmortyapi.com/api/character') => {
    loading.value = true
    try {
      const response = await fetch(url)
      const data = await response.json()
      //console.log(data.results) // отвал изображения
      characters.value = data.results
      pagination.value.next = data.info.next
      pagination.value.prev = data.info.prev
    } catch (error) {
      console.error('Error fetching characters:', error)
    } finally {
      loading.value = false
    }
  }

  const applyFilters = () => {
    const { name, status } = filters.value
    let url = `https://rickandmortyapi.com/api/character/?name=${name}&status=${status}`
    fetchCharacters(url)
  }

  onMounted(() => {
    fetchCharacters()
  })
</script>

<style scoped>
  body {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f5f5f5;
  }

  /* доп стили */
  h1 {
    text-align: center;
    margin: 20px 0;
    color: #333;
  }

  .filters {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
  }

  .filters label {
    margin-right: 10px;
    color: #333;
  }

  .filters input, 
  .filters select {
    margin-right: 20px;
    padding: 8px;
    border-radius: 4px;
    border: 1px solid #ccc;
  }

  .filters button {
    padding: 8px 16px;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: #f5f5f5;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .filters button:hover {
    background-color: #0056b3;
  }

  .character-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }

  .loading {
    text-align: center;
    font-size: 1.5em;
    color: #333;
  }

  .pagination {
    display: flex;
    justify-content: center;
    margin: 20px 0;

    width: 100%;
  }

  .pagination button {
    padding: 10px 20px;
    margin: 0 10px;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: #f5f5f5;
    cursor: pointer;
    transition: background-color 0.3s;

  }

  .pagination button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }

  .pagination button:hover:not(:disabled) {
    background-color: #0056b3;

  }
</style>