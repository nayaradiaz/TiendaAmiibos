<script setup>
import { ref, onMounted } from 'vue';
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

// Función para agregar un producto al carrito
const addToCart = (item) => {
    const itemInCart = cart.value.find(product => product.tail === item.tail);

    if (itemInCart) {
        itemInCart.quantity += 1;
    } else {
        item.quantity = 1;
        cart.value.push(item);
    }

    localStorage.setItem("cart", JSON.stringify(cart.value));
};

// Función para eliminar un ítem del carrito
const removeItem = (tail) => {
    const index = cart.value.findIndex(item => item.tail === tail);
    if (index !== -1) {
        if (cart.value[index].quantity > 1) {
            cart.value[index].quantity--;
        } else {
            cart.value.splice(index, 1);
        }
        localStorage.setItem('cart', JSON.stringify(cart.value));
    }
};

// Función para aumentar la cantidad de un ítem en el carrito
const increaseQuantity = (tail) => {
    const item = cart.value.find(item => item.tail === tail);
    if (item) {
        item.quantity++;
        localStorage.setItem('cart', JSON.stringify(cart.value));
    }
};

// Función para mostrar el carrito
const showCart = () => {
    cart.value = JSON.parse(localStorage.getItem("cart")) || [];
    document.getElementById("offcanvas").classList.add("open");
};

// Función para cerrar el carrito
const closeCart = () => {
    document.getElementById("offcanvas").classList.remove("open");
};

// Llamada a la API para cargar los productos
api();
</script>

<template>
    <header>
        <div>
            <h1>Tienda Amiibos</h1>
        </div>
        <div>
            <button @click="showCart" class="cerrar" id="texto">
                <i class="fas fa-shopping-cart"></i> Ver carrito
            </button>
        </div>
    </header>

    <p v-if="array == null">Cargando...</p>
    <div v-else class="container">
        <Card v-for="item in array" :key="item.tail" :data="item" @add-to-cart="addToCart" />
    </div>

    <!-- Offcanvas para el carrito -->
    <div id="offcanvas" class="offcanvas">
        <div class="offcanvas-content">
            <div class="encabezado">
                <div>
                    <h2>Carrito</h2>
                </div>
                <div>
                    <button @click="closeCart" class="cerrar">Cerrar</button>
                </div>
            </div>
            <div class="cart-items">
                <div v-for="item in cart" :key="item.tail">
                    <div class="cart-item">
                        <div>
                            <img :src="item.image" alt="Amiibo" class="cart-item-img" />
                        </div>
                        <div>
                            <h4>{{ item.name }}</h4>
                        </div>
                        <div>
                            <p>{{ item.amiiboSeries }}</p>
                        </div>
                        <div>

                            <!-- Botón para eliminar la cantidad -->
                            <button @click="removeItem(item.tail)" class="remove-btn">−</button>
                        </div>
                        <div>
                            <p>{{ item.quantity }}</p>
                        </div>
                        <div>
                            <!-- Botón para aumentar la cantidad -->
                            <button @click="increaseQuantity(item.tail)" class="increase-btn">+</button>
                        </div>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
.remove-btn,
.increase-btn {
    background-color: transparent;
    border: none;
    font-size: 1.5rem;
    cursor: pointer;
    transition: color 0.3s;
}
.remove-btn{
    color: #ff4747;

}
.increase-btn{
    color: #348bf0;
}
.remove-btn:hover {
    color: #fd5050;
}

.increase-btn:hover {
    color: rgb(58, 94, 255);
}

/* Estilos generales para el carrito */
.cart-items {
    width: 80%;
    margin: 0 auto;
    margin-top: 20px;
    border-top: 1px solid #ddd;
    padding-top: 10px;
}

.encabezado {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    width: 100%;
}

.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
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

.agregar {
    background-color: #007bff;
    color: white;
    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
}

.agregar:hover {
    background-color: #0056b3;
}

/* Estilos para el offcanvas */
.offcanvas {
    position: fixed;
    top: 0;
    right: -500px;
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
}

.offcanvas-content {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

.cart-item {
    display: flex;
    margin-bottom: 10px;
    justify-content: space-between;
    align-items: center;
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

.cerrar {
    background-color: #e60000;
    border: none;
    color: white;
    padding: 10px;
    cursor: pointer;
    border-radius: 5px;
}

.cerrar:hover {
    background-color: #b30000;
}

/* Media Queries para pantallas pequeñas */
@media (max-width: 768px) {
    .offcanvas {
        width: 100%;
        right: -150%;
    }

    .offcanvas.open {
        right: 0;
    }

    .offcanvas-content {
        width: 100%;
        padding: 20px;
    }

    .container {
        grid-template-columns: 1fr;
    }
}

/* Media Queries para tabletas */
@media (min-width: 768px) and (max-width: 1400px) {
    .container {
        grid-template-columns: repeat(2, 1fr);
    }
}
</style>
