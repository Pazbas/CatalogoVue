<template>
  <div class="modal fade show d-block" tabindex="-1">
    <div class="modal-dialog">
      <div class="modal-content">
        <img :src="producto.imagen" class="card-img-top" />

        <div class="modal-body">
          <h3>{{ producto.nombre }}</h3>
          <p>{{ producto.descripcion }}</p>
          <p><strong>Precio:</strong> ${{ producto.precio }}</p>
        </div>

        <div class="modal-footer">
          <button class="btn btn-secondary" @click="$emit('cerrar')">Cerrar</button>
        </div>
      </div>
    </div>
  </div>

  <!-- Fondo oscuro -->
  <div class="modal-backdrop fade show"></div>
</template>

<script setup>
defineProps({
  producto: Object
});
</script>
