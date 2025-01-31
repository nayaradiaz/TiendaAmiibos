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

// Carrito como estado reactivo
const cart = ref(JSON.parse(localStorage.getItem("cart")) || []);

// Función para agregar un producto al carrito
const addToCart = (item) => {
    const itemInCart = cart.value.find(product => product.tail === item.tail);

    if (itemInCart) {
        // Si el producto ya está en el carrito, aumentar su cantidad
        itemInCart.quantity += 1;
    } else {
        // Si el producto no está en el carrito, agregarlo con cantidad 1
        item.quantity = 1;
        cart.value.push(item);
    }

    // Guardar el carrito en el localStorage
    localStorage.setItem("cart", JSON.stringify(cart.value));
};
// Función para eliminar un ítem del carrito
const removeItem = (tail) => {
    const index = cart.value.findIndex(item => item.tail === tail);
    if (index !== -1) {
        // Si la cantidad es mayor que 1, reducimos la cantidad
        if (cart.value[index].quantity > 1) {
            cart.value[index].quantity--;
        } else {
            // Si la cantidad es 1, eliminamos el ítem
            cart.value.splice(index, 1);
        }

        // Actualizamos LocalStorage con el carrito actualizado
        localStorage.setItem('cart', JSON.stringify(cart.value));
    }
};

// Mostrar el carrito
const showCart = () => {
    cart.value = JSON.parse(localStorage.getItem("cart")) || [];
    document.getElementById("offcanvas").classList.add("open");
};

// Cerrar el carrito
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
            <button @click="showCart" class="cerrar" id="texto">
                <i class="fas fa-shopping-cart"></i> Ver carrito
            </button>
        </div>
    </header>

    <p v-if="array == null">Cargando...</p>
    <div v-else class="container">
        <Card v-for="item in array" :key="item.tail" :data="item" @add-to-cart="addToCart" />
    </div>

    <!-- Offcanvas -->
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
                        <div> <img :src="item.image" alt="Amiibo" class="cart-item-img" />
                        </div>
                        <div>
                            <h4>{{ item.name }}</h4>
                        </div>
                        <div>
                            <p>{{ item.amiiboSeries }}</p>
                        </div>
                        <div>
                            <p>{{ item.quantity }}</p>
                        </div>
                        <div>
                            <button @click="removeItem(item.tail)" class="remove-btn">−</button>
                        </div>
                    </div>
                </div>

            </div>


        </div>
    </div>
</template>



<style>
#texto{
    font-size: 18px;
}
.remove-btn {
    background-color: transparent;
    color: #ff4747;
    border: none;
    font-size: 1.5rem;
    cursor: pointer;
    transition: color 0.3s;
}

/* Hover para el botón de eliminar */
.remove-btn:hover {
    color: #fd5050;
}

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
