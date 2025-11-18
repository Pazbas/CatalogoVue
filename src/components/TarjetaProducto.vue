<template>
  <div class="card shadow-sm h-100" :class="{ 'opacity-50': stock === 0 }">
    <img :src="imagen" class="card-img-top" />

    <div class="card-body">
      <h5 class="card-title">{{ nombre }}</h5>
      <p class="card-text">{{ descripcion }}</p>

      <p class="mb-1">
        <strong>Precio:</strong> ${{ precio }}
      </p>

      <p :class="stock === 0 ? 'text-danger fw-bold' : 'text-success fw-bold'">
        {{ stock > 0 ? `Stock: ${stock}` : "Sin stock" }}
      </p>

      <slot></slot>

      <div class="d-flex gap-2 mt-3">
        <button class="btn btn-primary w-50" @click="$emit('ver-detalle')">
          Ver detalle
        </button>

        <button
          class="btn btn-success w-50"
          :disabled="stock === 0"
          @click="$emit('agregar-carrito', id)"
        >
          Agregar
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, onBeforeMount, onUnmounted } from "vue";

const props = defineProps({
  id: Number,
  nombre: String,
  precio: Number,
  imagen: String,
  stock: Number,
  descripcion: String
});


onBeforeMount(() => {
  console.log(`(created) Tarjeta de ${props.nombre} inicializada`);
});

onMounted(() => {
  console.log(`(mounted) Tarjeta de ${props.nombre} montada`);
});

onUnmounted(() => {
  console.log(`(destroyed) Tarjeta de ${props.nombre} destruida`);
});
</script>
