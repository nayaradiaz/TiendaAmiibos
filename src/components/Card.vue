<script setup>
defineProps({
    data: Object
})

// Función para añadir al carrito
const addToCart = () => {
    // Obtener carrito desde LocalStorage
    let cart = JSON.parse(localStorage.getItem("cart")) || [];
    
    // Verificar si el producto ya está en el carrito
    const productIndex = cart.findIndex(item => item.tail === data.tail);
    
    if (productIndex === -1) {
        // Si el producto no está en el carrito, lo agregamos
        cart.push(data);
    } else {
        // Si ya existe, actualizamos la cantidad o cualquier otra lógica
        cart[productIndex].quantity++;
    }

    // Guardamos el carrito actualizado en LocalStorage
    localStorage.setItem("cart", JSON.stringify(cart));

    console.log(`${data.name} added to cart`);
}
</script>

<template>
    <div class="wrapper">
        <div>
            <img :src="data.image" alt="Amiibo" class="img">
        </div>
        <div class="contenido">
            <h2>{{ data.name }}</h2>
            <p>{{ data.amiiboSeries }}</p>
            <button @click="addToCart">Add to Cart</button>
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
