<template>
    <div class="modal-overlay" v-if="isOpen">
        <div class="modal-content search-modal">
            <h2>🔍 Buscar Libro</h2>
        <div class="search-controls">
<!-- Selector de criterio de búsqueda -->
        <select v-model="searchType" class="search-select">
            <option value="title">Nombre del Libro</option>
            <option value="author">Autor</option>
            <option value="category">Categoría</option>
        </select>

        <input 
        v-model="searchQuery" 
        type="text" 
        placeholder="Escribe para buscar..." 
        @input="performSearch" 
        />
        </div>

    <div class="results-container">
<!-- MENSAJE SI NO HAY BÚSQUEDA ACTIVA -->
        <p v-if="!searchQuery" class="info-msg">Ingresa un término para comenzar la búsqueda.</p>

<!-- MENSAJE SI NO SE ENCONTRARON RESULTADOS -->
        <p v-else-if="filteredBooks.length === 0" class="error-msg">
            ❌ No se encontró ningún libro en la {{ searchType === 'title' ? 'biblioteca' : 'categoría/autor' }}.
        </p>

<!-- LISTA DE RESULTADOS -->
        <div v-else class="books-results">
            <div v-for="book in filteredBooks" :key="book.id" class="book-card">
            <strong>{{ book.title }}</strong><br>
            <small>Autor: {{ book.author }} | Categoría: {{ book.category }}</small>
            </div>
        </div>
    </div>

    <div class="modal-actions">
        <button @click="$emit('close')" class="btn-cancel">Cerrar</button>
    </div>
    </div>
</div>
</template>

<script setup>
import { ref, computed } from 'vue';

// 1. DEFINIMOS LAS PROPS UNA SOLA VEZ Y LAS ASIGNAMOS A UNA CONSTANTE
// Ahora 'props' contiene todo lo que viene del padre
const props = defineProps(['isOpen', 'books']);

// 2. DEFINIMOS LOS EVENTOS
defineEmits(['close']);

// 3. ESTADOS LOCALES
const searchQuery = ref('');
const searchType = ref('title');

// 4. LÓGICA DE BÚSQUEDA
const filteredBooks = computed(() => {
const query = searchQuery.value.toLowerCase().trim();

// Si el campo está vacío, devolvemos una lista vacía
if (!query) return [];

// IMPORTANTE: Aquí usamos 'props.books' porque definimos la constante arriba
return props.books.filter(book => {
    const valueToCompare = book[searchType.value].toLowerCase();
    return valueToCompare.includes(query);
});
});

// Ya no necesitamos la línea de defineProps al final, ¡borrada!
</script>

<style scoped>
.modal-overlay {
    position: fixed;
    top: 0; left: 0; width: 100%; height: 100%;
    background: rgba(0, 0, 0, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
}
.modal-content {
    background: white;
    padding: 2rem;
    border-radius: 12px;
    width: 500px;
    max-height: 80vh;
    display: flex;
    flex-direction: column;
}
.search-controls {
    display: flex;
    gap: 10px;
    margin-bottom: 1.5rem;
}
.search-select {
    padding: 10px;
    border-radius: 6px;
    border: 1px solid #ddd;
    cursor: pointer;
}
input {
    flex: 1;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 6px;
}
.results-container {
    flex: 1;
    overflow-y: auto;
    min-height: 200px;
    border: 1px solid #eee;
    border-radius: 8px;
    padding: 1rem;
    background: #fafafa;
}
.book-card {
    background: white;
    padding: 10px;
    border-radius: 6px;
    margin-bottom: 10px;
    border-left: 4px solid #8e44ad;
    box-shadow: 0 2px 5px rgba(0,0,0,0.05);
}
.info-msg { color: #666; text-align: center; font-style: italic; }
.error-msg { color: #e74c3c; text-align: center; font-weight: bold; }
.modal-actions {
    display: flex;
    justify-content: flex-end;
    margin-top: 1.5rem;
}
.btn-cancel { background: #95a5a6; color: white; border: none; padding: 10px 15px; border-radius: 6px; cursor: pointer; }
</style>