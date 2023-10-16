<template>
    <div>
        <h2>Lista de Notas</h2>
        <div v-if="notas.length === 0">No hay notas.</div>
        <div v-else>
            <div v-for="(nota, index) in notas" :key="index" class="nota">
                <h3>Titulo: {{ nota.titulo }}</h3>
                <h4>Descripción: {{ nota.contenido }}</h4>
                <button class="buttonAceptar" @click="abrirModalEdicion(index)">Editar</button>
                <button class="buttonCancelar" @click="eliminar(index)">Eliminar</button>
            </div>
        </div>

        <!-- Modal de Edición -->
    <div class="modal" v-show="modalVisible">
      <div class="modal-content">
        <span class="close" @click="cerrarModalEdicion">&times;</span>
        <h3>Editar Nota</h3>
        <div>
          <label for="nuevoTitulo">Nuevo Título:</label>
          <input v-model="nuevoTitulo" type="text" id="nuevoTitulo">
        </div>
        <div>
          <label for="nuevoContenido">Nuevo Contenido:</label>
          <textarea v-model="nuevoContenido" id="nuevoContenido"></textarea>
        </div>
        <button @click="guardarEdicion">Guardar</button>
      </div>
    </div>


    </div>
</template>

<script setup>
import { defineProps, ref } from 'vue';
//import { defineProps } from 'vue';

const props = defineProps({
    notas: {
        type: Array,
        default: []
    }
})

const emits = defineEmits(['eliminarNota', 'editarNota'])

const modalVisible = ref(false);
const nuevoTitulo = ref('');
const nuevoContenido = ref('');
let notaEditadaIndex = -1;

function abrirModalEdicion(index) {
  const infoNota = { ...props.notas[index] };
  nuevoTitulo.value = infoNota.titulo;
  nuevoContenido.value = infoNota.contenido;
  notaEditadaIndex = index;
  modalVisible.value = true;
}

function cerrarModalEdicion() {
  modalVisible.value = false;
}

function guardarEdicion() {
  const nuevaInfo = { titulo: nuevoTitulo.value, contenido: nuevoContenido.value };
  emits('editarNota', notaEditadaIndex, nuevaInfo);
  cerrarModalEdicion();
}

function eliminar(index) {
        emits('eliminarNota', index);
    
}
</script>
  
<style scoped>
.nota {
    border: 1px solid #ccc;
    padding: 10px;
    margin: 10px 0;
}
.modal {
  display: none;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0,0,0,0.4);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content {
  background-color: #fff;
  padding: 20px;
  border-radius: 4px;
  width: 30%;
  display: flex; 
  flex-direction: column;
}
.close {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 20px;
  cursor: pointer;
}
.modal-content label {
  display: block;
  margin-top: 10px;
}
.modal-content input, .modal-content textarea {
  width: 70%;
  padding: 10px;
  margin: 5px 0;
  border: 1px solid #ccc;
  border-radius: 3px;

}
.modal-content button {
  background-color: #007BFF;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 3px;
  cursor: pointer;
  align-self: flex-end;
}
.buttonAceptar{
    background-color: #db2555;
    color: #fff;
    border: none;
    padding: 5px 10px;
    border-radius: 3px;
    cursor: pointer;
    
    
}
.buttonCancelar{
    background-color: red;
    color: #fff;
    border: none;
    padding: 5px 10px;
    border-radius: 3px;
    cursor: pointer;
    margin-left: 16px;

}
</style>
  