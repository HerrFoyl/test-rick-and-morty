<template>
  <div class="character-card">
      <img :src="character.image" alt="Caracter Image" />
      <h2>{{ character.name }}</h2>
      <p>Status: {{ character.status }}</p>
      <p>Species: {{ character.species }}</p>
      <p>Gernder {{ character.gender }}</p>
  </div>
</template>

<script setup>
  
  const props = defineProps({
      character: Object
  })
</script>

<style scoped>
.character-card {
  border: 1px solid #ccc;
  padding: 16px;
  margin: 16px;
  border-radius: 8px;
  text-align: center;
  /* доп стили */
  background-color: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
}


.character-card img {
  max-width: 100%;
  border-radius: 50%;
  margin-bottom: 16px;
}

/* доп стили */
character-card:hover {
  transform: scale(1.05);
}
.character-card h2 {
  margin: 0;
  font-size: 1.5em;
  color: #333;
}
.character-card p {
  margin: 8px 0;
  color: #555;
}
.character-card p strong {
  color: #333;
}
</style>