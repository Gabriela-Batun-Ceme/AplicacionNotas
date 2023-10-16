<template>
  <div>
    <h2>Agregar Nueva Nota</h2>
    <form @submit.prevent="agregar(this)">
      <input v-model="nuevaNota.titulo" placeholder="Título" required />
      <textarea v-model="nuevaNota.contenido" placeholder="Contenido" required ></textarea>
      <button type="submit">Guardar Nota</button>
    </form>
  </div>
</template>
  
<script setup>
import { reactive } from 'vue';

let nuevaNota = reactive({ titulo: '', contenido: '' })

function agregar(cntx) {
  cntx.$emit('nuevaNota', { ...nuevaNota });
  nuevaNota.titulo = '';
  nuevaNota.contenido = '';
}

</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}

.input{
margin-top: 15px;
}

button {
  background-color: #007BFF;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 3px;
  cursor: pointer;
  align-self: flex-end;
  margin-top: 15px;
}


textarea {
  margin-top: 15px;
  width: 30vw;
  height: 15vh;
  
}

</style>
  