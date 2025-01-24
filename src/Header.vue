<script setup>

</script>

<template>
  <div>
    <h1>Tienda Amiibos</h1>

  </div>
  <div>
    <input type="text" name="" class="input">
    <input type="submit" value="Buscar" id="button">
  </div>


</template>
<style scope>
header {
  background-color: rgb(255, 72, 72);
  color: white;
  height: 10vh;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.input {
  border: 2px solid transparent;
  width: 15em;
  height: 2.5em;
  padding-left: 0.8em;
  outline: none;
  overflow: hidden;
  background-color: #F3F3F3;
  border-radius: 10px;
  transition: all 0.5s;
}

.input:hover,
.input:focus {
  border: 2px solid #4A9DEC;
  box-shadow: 0px 0px 0px 7px rgb(74, 157, 236, 20%);
  background-color: white;
}

/* From Uiverse.io by adamgiebl */ 
#button {
  color: #090909;
  padding: 0.7em 1.7em;
  font-size: 18px;
  border-radius: 0.5em;
  background: #e8e8e8;
  cursor: pointer;
  border: 1px solid #e8e8e8;
  transition: all 0.3s;
  box-shadow: 6px 6px 12px #c5c5c5, -6px -6px 12px #ffffff;
}

#button:hover {
  border: 1px solid white;
}


#button:active {
  box-shadow: 4px 4px 12px #c5c5c5, -4px -4px 12px #ffffff;
}


h1 {
  margin: 0;
}
</style>