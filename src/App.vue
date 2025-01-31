<script setup>
import { ref } from 'vue';
import Card from './components/Card.vue';

let array = ref(null);
const url = "https://www.amiiboapi.com/api/amiibo/";

const api = async () => {
  try {
    const response = await fetch(url);
    const data = await response.json();
    array.value = data.amiibo;
  } catch (error) {
    console.error("Error fetching data: ", error);
  }
};

const cart = ref(JSON.parse(localStorage.getItem("cart")) || []);

const showCart = () => {
  cart.value = JSON.parse(localStorage.getItem("cart")) || [];
  document.getElementById("offcanvas").classList.add("open");
};

const closeCart = () => {
  document.getElementById("offcanvas").classList.remove("open");
};

api();
</script>

<template>
  <header>
    <div>
      <h1>Tienda Amiibos</h1>
    </div>
    <div>
      <button @click="showCart">Ver Carrito</button>
    </div>
  </header>

  <p v-if="array == null">Cargando...</p>
  <div v-else class="container">
    <Card v-for="item in array" :key="item.tail" :data="item" />
  </div>

  <!-- Offcanvas -->
  <div id="offcanvas" class="offcanvas">
    <div class="offcanvas-content">
      <h2>Carrito</h2>
      <ul>
        <li v-for="item in cart" :key="item.tail">
          <div class="cart-item">
            <img :src="item.image" alt="Amiibo" class="cart-item-img" />
            <div>
              <h4>{{ item.name }}</h4>
              <p>{{ item.amiiboSeries }}</p>
            </div>
          </div>
        </li>
      </ul>
      <button @click="closeCart">Cerrar</button>
    </div>
  </div>
</template>
<style>
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  /* 4 columnas por defecto */
  grid-template-rows: repeat(3, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 15px;
  justify-items: center;
  justify-content: center;
  align-content: center;
  margin: 0 auto;
  width: 70%;
  margin-top: 20px;

}

header {
  background-color: #dd2020;
  color: white;
  height: 10vh;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

button {
  background-color: #007bff;
  color: white;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

/* Offcanvas Styles */
.offcanvas {
  position: fixed;
  top: 0;
  right: -500px;
  /* Initially hidden off-screen */
  width: 400px;
  height: 100%;
  background-color: #fff;
  box-shadow: -2px 0px 5px rgba(0, 0, 0, 0.3);
  transition: right 0.3s ease;
  padding: 20px;
  overflow-y: auto;
  z-index: 9999;
}

.offcanvas.open {
  right: 0;
  /* Show the offcanvas */
}

.offcanvas-content {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.cart-item {
  display: flex;
  margin-bottom: 10px;
}

.cart-item-img {
  width: 50px;
  height: 50px;
  margin-right: 10px;
}

h2 {
  margin-top: 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

button {
  margin-top: 20px;
  background-color: #e60000;
  border: none;
  color: white;
  padding: 10px;
  cursor: pointer;
  border-radius: 5px;
}

button:hover {
  background-color: #b30000;
}

/* Media Queries para pantallas pequeñas (Móviles) */
@media (max-width: 768px) {
  .offcanvas {
    width: 100%;
    /* El offcanvas ocupará toda la pantalla */
    right: -150%;
    /* Lo oculta al principio */
  }

  .offcanvas.open {
    right: 0;
    /* Muestra el offcanvas a toda pantalla */
  }

  .offcanvas-content {
    width: 100%;
    padding: 20px;
  }

  /* Para el botón de cerrar en pantallas móviles */
  button {
    width: 100%;
    /* Hace que el botón ocupe todo el ancho */
  }

  /* Para móviles, mostramos solo 1 columna */
  .container {
    grid-template-columns: 1fr;
    /* 1 columna */
    grid-template-rows: auto;
  }
}

/* Media Queries para tabletas */
@media (min-width: 768px) and (max-width: 1400px) {

  /* Para tabletas, mostramos 2 columnas */
  .container {
    grid-template-columns: repeat(2, 1fr);
    /* 2 columnas */
  }
}
</style>
