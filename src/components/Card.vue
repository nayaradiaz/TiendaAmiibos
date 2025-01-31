<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  data: Object
});

const emit = defineEmits();

const addToCart = () => {
  const item = props.data;  
  emit('add-to-cart', item); 
};
</script>

<template>
  <div class="tarjeta">
    <div class="img-container">
      <img :src="props.data.image" alt="Amiibo" class="img" />
    </div>
    <div class="info">
      <h3 class="titulo">{{ props.data.name }}</h3>
      <p class="serie">{{ props.data.amiiboSeries }}</p>
      <button @click="addToCart" class="btn-agregar">Añadir al carrito</button>
    </div>
  </div>
</template>

<style scoped>
.tarjeta {
  display: flex;
  flex-direction: column;
  border-radius: 15px;
  background-color: #cacaca;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
  cursor: pointer;
  overflow: hidden;
  width: 300px;
  height: 400px;
  margin: 15px;
  text-align: center;
}

.tarjeta:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
}

.img-container {
  width: 100%;
  height: 65%;
  overflow: hidden;
}

.img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease-in-out;
}

.img-container:hover .img {
  transform: scale(1.1);
}

.info {
  padding: 20px;
}

.titulo {
  font-size: 1.25rem;
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

.serie {
  font-size: 1rem;
  color: #777;
  margin-bottom: 20px;
}

.btn-agregar {
  padding: 12px 20px;
  background-color: #007bff;
  color: white;
  font-size: 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

.btn-agregar:hover {
  background-color: #0056b3;
}
</style>
