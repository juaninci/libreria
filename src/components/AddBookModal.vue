<template>
<div class="modal-overlay" v-if="isOpen">
    <div class="modal-content">
        <h2> &#x1f70a Añadir Nuevo Libro</h2>
            <form @submit.prevent="handleSubmit">
        <div class="form-group">
            <label>Nombre del Libro</label>
            <input v-model="newBook.title" type="text" required placeholder="Ej: Don Quijote" />
        </div>

        <div class="form-group">
            <label>Autor</label>
            <input v-model="newBook.author" type="text" required placeholder="Ej: Cervantes" />
        </div>

        <div class="form-group">
            <label>Categoría</label>
            <input v-model="newBook.category" type="text" required placeholder="Ej: Clásico" />
        </div>

        <div class="modal-actions">
            <button type="button" @click="$emit('close')" class="btn-cancel">Cancelar</button>
            <button type="submit" class="btn-save">Guardar Libro</button>
    </div>
    </form>
    </div>
</div>
</template>

<script setup>
import { ref } from 'vue';

defineProps(['isOpen']);
const emit = defineEmits(['close', 'add-book']);

const newBook = ref({ title: '', author: '', category: '' });

const handleSubmit = () => {
  // Enviamos una copia de los datos al padre
    emit('add-book', { ...newBook.value });
  // Reset form
    newBook.value = { title: '', author: '', category: '' };
};
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
    width: 400px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.2);
}
.form-group {
    margin-bottom: 1rem;
    display: flex;
    flex-direction: column;
}
.form-group label { font-weight: bold; margin-bottom: 5px; }
.form-group input { padding: 10px; border: 1px solid #ddd; border-radius: 6px; }
.modal-actions {
    display: flex;
    justify-content: flex-end;
    gap: 10px;
    margin-top: 1.5rem;
}
.btn-cancel { background: #95a5a6; color: white; border: none; padding: 10px 15px; border-radius: 6px; cursor: pointer; }
.btn-save { background: #000000; color: rgb(255, 255, 255); border: none; padding: 10px 15px; border-radius: 6px; cursor: pointer; }
</style>