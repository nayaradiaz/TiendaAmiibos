<script setup>
defineProps({
    data: Object
})
</script>

<template>

    <div class="wrapper">
        <!-- <p>{{ data }}</p> -->
        <div>
            <img :src="data.image" alt="Amiibo" class="img">

        </div>
        <div class="contenido">
            <h2>{{ data.name }}</h2>
            <p>{{ data.amiiboSeries }}</p>
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

.contenido {
    text-align: center;
    width: 15vh;
}

.img {
    width: 25vh;
    height: 35vh;
}
</style>