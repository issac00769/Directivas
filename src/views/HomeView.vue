<script setup lang="ts">
import { ref } from 'vue'

const nombre = ref('');
const apellido = ref('');
const edad = ref(0);
const genero = ref('');
const enviado = ref(false);
const opcionesGenero = ['masculino', 'femenino'];
const otroGenero = ref('');

const validarFormulario = () => {
  const nombreValido = nombre.value.length >= 5 && nombre.value.length <= 18;
  const apellidoValido = apellido.value !== nombre.value;
  const edadValida = edad.value > 0 && edad.value < 60;
  return nombreValido && apellidoValido && edadValida;
  
}

const agreGenero = () => {
  if (genero.value !== 'otro') {
    otroGenero.value = '';
  }
}

const submitForm = () =>{
  enviado.value = true;
}
</script>

<template>
  <main class="main">
    <h1>Formulario de Registro</h1>
    <form @submit.prevent="submitForm" class="formulario">
      <div class="campo">
        <label for="nombre" class="label">Nombre:</label>
        <input type="text" id="nombre" v-model="nombre" class="input">
        <span v-if="nombre.length < 5 || nombre.length > 18" class="mensaje-error">El nombre debe tener entre 5 y 18 caracteres.</span>
      </div>
      <div class="campo">
        <label for="apellido" class="label">Apellido:</label>
        <input type="text" id="apellido" v-model="apellido" class="input">
        <span v-if="apellido === nombre" class="mensaje-error">El apellido no puede ser igual al nombre.</span>
      </div>
      <div class="campo">
        <label for="edad" class="label">Edad:</label>
        <input type="number" id="edad" v-model.number="edad" class="input">
        <span v-if="edad <= 0 || edad >= 60" class="mensaje-error">La edad debe ser mayor a 0 y menor a 60.</span>
      </div>
      <div class="campo">
        <label for="genero" class="label">Género:</label>
        <select id="genero" v-model="genero" @change="agreGenero" class="input">
          <option v-for="opcion in opcionesGenero" :key="opcion" :value="opcion">{{ opcion }}</option>
          <option value="otro">Otro</option>
        </select>
        <!-- Campo de texto adicional para otro género -->
        <input v-if="genero === 'otro'" type="text" id="otroGenero" v-model="otroGenero" class="input" placeholder="Especificar otro género">
      </div>
      <button type="submit" :disabled="!validarFormulario()" class="boton">Enviar</button>
    </form>
    <!-- Mensaje de confirmación -->
    <div v-if="enviado" class="mensaje-confirmacion">
      ¡El formulario se ha enviado correctamente!
    </div>
  </main>
</template>

<style scoped>
/* Estilos generales */
.main {
  font-family: 'Arial', sans-serif;
  padding: 20px;
  background-color: #f0f0f0;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
  color: #333;
  text-align: center;
}

.input {
  width: 100%;
  padding: 12px;
  font-size: 16px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-sizing: border-box;
}

.formulario {
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 8px;
  background-color: #fff;
  margin: 0 auto;
  margin-top: 50px;
}

.campo {
  margin-bottom: 20px;
}

.label {
  font-weight: bold;
  display: block;
  color: #333;
}

.boton {
  background-color: #4caf50;
  color: #fff;
  padding: 12px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  display: block;
  margin: 0 auto;
}

.boton:hover {
  background-color: #45a049;
}

.mensaje-error {
  color: #d9534f;
  font-size: 14px;
  display: block;
  margin-top: 5px;
}

.mensaje-confirmacion {
  color: #5bc0de;
  font-size: 18px;
  margin-top: 20px;
  font-weight: bold;
}

/* Estilos responsivos */
@media (max-width: 600px) {
  .formulario {
    max-width: 100%;
  }

  .campo {
    margin-bottom: 15px;
  }

  .boton {
    width: 100%;
  }
}
</style>
