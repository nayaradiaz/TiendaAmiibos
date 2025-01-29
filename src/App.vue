<script setup>

import { ref } from 'vue'
import Card from './components/Card.vue';

let array = ref(null);
const url = "https://www.amiiboapi.com/api/amiibo/";
const api = async () => {
    await fetch(url)
        .then(response => response.json())
        .then(data => {
            console.log(data);
            array.value = data.amiibo;
        })

}
api();
</script>

<template>
    <p v-if="array == null">Cragando...</p>
    <div v-else class="container">

        <Card v-for="item in array" :data=item></Card>
    </div>
</template>
<style>
.container {
    margin-top: 20px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(4, 1fr);
    grid-column-gap: 0px;
    grid-row-gap: 15px;
    justify-items: center;
    justify-content: center;
    align-content: center;
    
}
</style>
