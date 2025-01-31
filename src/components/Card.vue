<script setup>
import { defineProps, defineEmits } from 'vue';

// Definir las props que recibimos
const props = defineProps({
  data: Object
});

// Definir la función para emitir el evento 'add-to-cart'
const emit = defineEmits();

// Emitir el producto cuando se hace clic en "Añadir al carrito"
const addToCart = () => {
  const item = props.data;  // Accedemos a los datos pasados por el padre
  emit('add-to-cart', item); // Emitir el evento con el producto
};


</script>

<template>
  <div class="wrapper">
    <div>
      <img :src="props.data.image" alt="Amiibo" class="img">
    </div>
    <div class="contenido">
      <h2>{{ props.data.name }}</h2>
      <p>{{ props.data.amiiboSeries }}</p>
      <button @click="addToCart" class="agregar">Añadir al carrito</button>
    </div>
  </div>
</template>


<style scoped>
.wrapper {
    display: flex;
    border-radius: 5px;
    box-shadow: #a5a2a2 5px 5px 10px;
    background-color: #e5e5e5;
    width: 40vh;
    height: 35vh;
}

.wrapper:hover {
    transition: ease 1s;
    transform: scale(0.95);
}

button {
    margin-top: 10px;
    padding: 10px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}

.img {
    width: 25vh;
    height: 35vh;
}

.contenido {
    text-align: center;
    width: 15vh;
}
</style>
