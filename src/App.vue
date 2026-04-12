<template>
  <div class="app-container">
    <!-- Control del modal mediante una prop y listening del evento de añadir -->
    <AddBookModal 
      :isOpen="isModalOpen" 
      @close="isModalOpen = false" 
      @add-book="handleAddBook" 
    />
    <header class="header">
      <h1>&#x1f5fa  Gestor de Libros</h1>
      <div class="header-buttons">
        <!-- BOTÓN PARA BUSCAR -->
        <button @click="isSearchOpen = true" class="btn-search">Buscar</button>
        <button @click="isModalOpen = true" class="btn-add">Añadir Libro</button>
      </div>
    </header>
        <Libros :books="books" @delete-book="confirmDelete" @update-book="saveEdit" />

    <!-- COMPONENTE DE BÚSQUEDA -->
    <Busqueda :isOpen="isSearchOpen" :books="books" @close="isSearchOpen = false" />

  </div>
</template>



<script setup>
import { ref } from 'vue';
import Libros from './components/libros.vue';
import AddBookModal from './components/AddBookModal.vue';
import Busqueda from './components/Busqueda.vue'; // Importar componente

// ... otros estados ...
const isSearchOpen = ref(false); // Estado para el modal de búsqueda

// ESTADO GLOBAL: La única fuente de verdad de los datos
const books = ref([
  { id: 1, title: 'Cien años de soledad', author: 'Gabriel García Márquez', category: 'Realismo Mágico' },
  { id: 2, title: '1984', author: 'George Orwell', category: 'Distopía' },
  { id: 3, title: 'El Hobbit', author: 'J.R.R. Tolkien', category: 'Fantasía' },
  { id: 4, title: 'Harry Potter y la Piedra Filosofal', author: 'J.K. Rowling', category: 'Fantasía' },
  { id: 5, title: 'Harry Potter y la Cámara Secreta', author: 'J.K. Rowling', category: 'Fantasía' },
  { id: 6, title: 'Harry Potter y el Prisionero de Azkaban', author: 'J.K. Rowling', category: 'Fantasía' },
  { id: 7, title: 'Harry Potter y el Cáliz de Fuego', author: 'J.K. Rowling', category: 'Fantasía' },
  { id: 8, title: 'Harry Potter y la Orden del Fénix', author: 'J.K. Rowling', category: 'Fantasía' },
  { id: 9, title: 'Harry Potter y el Misterio del Príncipe', author: 'J.K. Rowling', category: 'Fantasía' },
  { id: 10, title: 'Harry Potter y las Reliquias de la Muerte', author: 'J.K. Rowling', category: 'Fantasía' },
  { id: 11, title: 'La Comunidad del Anillo', author: 'J.R.R. Tolkien', category: 'Fantasía' },
  { id: 12, title: 'Las Dos Torres', author: 'J.R.R. Tolkien', category: 'Fantasía' },
  { id: 13, title: 'El Retorno del Rey', author: 'J.R.R. Tolkien', category: 'Fantasía' },
  { id: 14, title: 'Dune', author: 'Frank Herbert', category: 'Ciencia Ficción' },
  { id: 15, title: 'El Mesías de Dune', author: 'Frank Herbert', category: 'Ciencia Ficción' },
  { id: 16, title: 'Hijos de Dune', author: 'Frank Herbert', category: 'Ciencia Ficción' },
  { id: 17, title: 'El Emperador Dios de Dune', author: 'Frank Herbert', category: 'Ciencia Ficción' },
  { id: 18, title: 'Herejes de Dune', author: 'Frank Herbert', category: 'Ciencia Ficción' },
  { id: 19, title: 'El Dios Emperador de Dune', author: 'Frank Herbert', category: 'Ciencia Ficción' },
  { id: 20, title: 'It', author: 'Stephen King', category: 'Terror' },
  { id: 21, title: 'El Resplandor', author: 'Stephen King', category: 'Terror' },
  { id: 22, title: 'Misery', author: 'Stephen King', category: 'Terror' },
  { id: 23, title: 'Carrie', author: 'Stephen King', category: 'Terror' },
  { id: 24, title: 'La Torre Oscura I: El Pistolero', author: 'Stephen King', category: 'Fantasía Oscura' },
  { id: 25, title: 'La Torre Oscura II: La Llamada de los Tres', author: 'Stephen King', category: 'Fantasía Oscura' },
  { id: 26, title: 'La Torre Oscura III: Las Tierras Baldías', author: 'Stephen King', category: 'Fantasía Oscura' },
  { id: 27, title: 'La Torre Oscura IV: Magia y Cristal', author: 'Stephen King', category: 'Fantasía Oscura' },
  { id: 28, title: 'La Torre Oscura V: Los Lobos de Calla', author: 'Stephen King', category: 'Fantasía Oscura' },
  { id: 29, title: 'La Torre Oscura VI: El Song of Susannah', author: 'Stephen King', category: 'Fantasía Oscura' },
  { id: 30, title: 'La Torre Oscura VII: La Torre Oscura', author: 'Stephen King', category: 'Fantasía Oscura' },
  { id: 31, title: 'Horus Rising', author: 'Dan Abnett', category: 'Grimdark' },
  { id: 32, title: 'False Gods', author: 'Dan Abnett', category: 'Grimdark' },
  { id: 33, title: 'Fulgrim', author: 'Dan Abnett', category: 'Grimdark' },
  { id: 34, title: 'Gal Vorbaidmente', author: 'Aaron Dembski-Bowden', category: 'Grimdark' },
  { id: 35, title: 'Eisenhorn: Xenos', author: 'Dan Abnett', category: 'Grimdark' },
  { id: 36, title: 'Eisenhorn: Malleus', author: 'Dan Abnett', category: 'Grimdark' },
  { id: 37, title: 'Eisenhorn: Hunter', author: 'Dan Abnett', category: 'Grimdark' },
  { id: 38, title: 'First and Only', author: 'Dan Abnett', category: 'Grimdark' },
  { id: 39, title: 'Ciaphas Cain: For the Emperor', author: 'Sandy Mitchell', category: 'Comedia Negra' },
  { id: 40, title: 'The Infinite and the Divine', author: 'Robert M. Price', category: 'Ciencia Ficción' },
  { id: 41, title: 'Night Lords', author: 'Aaron Dembski-Bowden', category: 'Grimdark' },
  { id: 42, title: 'The End and the Death', author: 'Dan Abnett', category: 'Grimdark' },
]);

const isModalOpen = ref(false);

// LÓGICA PARA AÑADIR LIBRO
const handleAddBook = (bookData) => {
  const newBook = {
    id: Date.now(), 
    ...bookData
  };
  books.value.push(newBook);
  isModalOpen.value = false; 
};

// LÓGICA PARA ELIMINAR libro
const confirmDelete = (id) => {
  if (confirm('¿Estás seguro de que deseas eliminar este libro?')) {
    books.value = books.value.filter(book => book.id !== id);
  }
};

// LÓGICA PARA ACTUALIZAR
const saveEdit = (updatedBook) => {
  const index = books.value.findIndex(b => b.id === updatedBook.id);
  if (index !== -1) {
    books.value[index] = updatedBook;
  }
};
</script>




<style>
.header-buttons {
  display: flex;
  gap: 10px;
}
.btn-search {
  background-color: #ffffff; 
  color: #333;               
  padding: 12px 20px;
  border: 1px solid #ddd;    
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  font-size: 1rem;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); 
  transition: all 0.3s ease; 
}
.btn-search:hover {
  background-color: #f8f9fa;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15); 
  transform: translateY(-1px); 
}
.btn-search:active {
  transform: translateY(0);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}
.app-container {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  max-width: 1000px;
  margin: 0 auto;
  padding: 2rem;
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}
.btn-add {
  background-color: #000000; 
  color: #ffffff;            
  padding: 12px 20px;
  border: 1px solid #000000; 
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  font-size: 1rem;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); 
  
  transition: all 0.3s ease;
}
.btn-add:hover {
  background-color: #333333; 
  color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3); 
  transform: translateY(-1px); 
}
.btn-add:active {
  transform: translateY(0);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
}
</style>