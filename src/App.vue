<template>
  <div class="container py-4">
    <CatalogoProductos />
  </div>
</template>

<script setup>
import CatalogoProductos from "./components/CatalogoProductos.vue";
</script>

<style>
.bg-fondo {
  background-image: url('../src/assets/img/fondo.jpg'); 
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}
</style>
