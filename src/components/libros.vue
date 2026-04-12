<template>
    <table class="book-table">
        <thead>
    <tr>
        <th>Nombre</th>
        <th>Autor</th>
        <th>Categoría</th>
        <th>Acciones</th>
    </tr>
    </thead>
    <tbody>
        <tr v-for="book in books" :key="book.id">
        <!-- Nombre -->
        <td>
            <input v-if="editingId === book.id" v-model="editForm.title" type="text" />
            <span v-else>{{ book.title }}</span>
        </td>
        <!-- Autor -->
        <td>
            <input v-if="editingId === book.id" v-model="editForm.author" type="text" />
            <span v-else>{{ book.author }}</span>
        </td>
        <!-- Categoría -->
        <td>
            <input v-if="editingId === book.id" v-model="editForm.category" type="text" />
            <span v-else>{{ book.category }}</span>
        </td>
        <!-- Acciones -->
        <td>
            <div class="actions">
            <button v-if="editingId !== book.id" @click="startEdit(book)" class="btn-edit">&#x21c8</button>
            <button v-else @click="emitSave(book.id)" class="btn-save">&#x21ca</button>
            
            <button @click="$emit('delete-book', book.id)" class="btn-delete">&#x21cc</button>
            
            <button v-if="editingId === book.id" @click="cancelEdit" class="btn-cancel">&#x21c4</button>
            </div>
        </td>
    </tr>
</tbody>
</table>
</template>

<script setup>
import { ref } from 'vue';

// input de los libros del padre
defineProps(['books']);
// Defnicion de los eventos que avisan al padre
const emit = defineEmits(['delete-book', 'update-book']);
const editingId = ref(null);
const editForm = ref({ title: '', author: '', category: '' });
const startEdit = (book) => {
    editingId.value = book.id;
    editForm.value = { ...book };
};

const cancelEdit = () => {
    editingId.value = null;
};

const emitSave = (id) => {
  // Enviar el libro modificado al padre
    emit('update-book', { ...editForm.value, id });
    editingId.value = null;
};
</script>

<style scoped>
.book-table {
    width: 100%;
    border-collapse: collapse;
    box-shadow: 0 2px 15px rgba(0,0,0,0.1);
}
.book-table th, .book-table td {
    border: 1px solid #eee;
    padding: 12px;
    text-align: left;
}
.book-table th { background-color: #f8f9fa; }
.actions { display: flex; gap: 8px; }

button {
    border: none;
    border-radius: 4px;
    padding: 5px 10px;
    cursor: pointer;
}

.btn-save { background-color: #2ecc71; color: white; }
.btn-cancel { background-color: #95a5a6; color: white; }

input {
    padding: 5px;
    border: 1px solid #3498db;
    border-radius: 4px;
    width: 90%;
}
.btn-edit {
    background-color: #000000;
    color: #ffffff;            
    padding: 6px 12px;         
    border: 1px solid #000000;
    border-radius: 6px;       
    cursor: pointer;
    font-weight: bold;
    font-size: 0.85rem;        
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2); 
    transition: all 0.3s ease;
}
.btn-edit:hover {
    background-color: #ffffff; 
    color: #252525;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3); 
    transform: translateY(-1px); 
}
.btn-edit:active {
    transform: translateY(0);
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
}
.btn-delete {
    background-color: #ffffff; 
    color: #000000;          
    padding: 6px 12px;        
    border: 1px solid #000000;
    border-radius: 6px;        
    cursor: pointer;
    font-weight: bold;
    font-size: 0.85rem;        
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2); 
    transition: all 0.3s ease;
}
.btn-delete:hover {
    background-color: #333333; 
    color: #ffffff;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3); 
    transform: translateY(-1px); 
}
.btn-delete:active {
    transform: translateY(0);
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
}
.btn-save {
    background-color: #535353;
    color: #ffffff;            
    padding: 6px 12px;         
    border: 1px solid #000000;
    border-radius: 6px;        
    cursor: pointer;
    font-weight: bold;
    font-size: 0.85rem;        
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2); 
    transition: all 0.3s ease;
}
.btn-save:hover {
    background-color: #333333; 
    color: #ffffff;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3); 
    transform: translateY(-1px); 
}
.btn-save:active {
    transform: translateY(0);
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
}
</style>