<template>
  <div>
     <h1>Tarea M7-AE1ABPRO</h1>
    <div class="d-flex justify-content-between align-items-center mb-4">
     
      
      <h2>Catálogo de productos</h2>

      
      <button class="btn btn-dark position-relative" @click="mostrarCarrito = true">
         Carrito
        <span class="badge bg-danger position-absolute top-0 start-100 translate-middle">
          {{ totalCantidad }}
        </span>
      </button>
    </div>

    <div class="row g-4">
      <div class="col-md-4" v-for="prod in productos" :key="prod.id">
        <TarjetaProducto
          :id="prod.id"
          :nombre="prod.nombre"
          :precio="prod.precio"
          :imagen="prod.imagen"
          :stock="prod.stock"
          :descripcion="prod.descripcion"
          @agregar-carrito="agregarProductoCarrito"
          @ver-detalle="productoSeleccionado = prod"
        >
          <span v-if="prod.stock < 5" class="badge bg-danger mt-2">
            🔥 Últimas unidades
          </span>
        </TarjetaProducto>
      </div>
    </div>

   
    <component
      v-if="productoSeleccionado"
      :is="DetalleProducto"
      :producto="productoSeleccionado"
      @cerrar="productoSeleccionado = null"
    />

   
    <div v-if="mostrarCarrito" class="modal fade show d-block" tabindex="-1">
      <div class="modal-dialog modal-lg">
        <div class="modal-content">

          <div class="modal-header">
            <h5 class="modal-title">Carrito de compras</h5>
            <button class="btn-close" @click="mostrarCarrito = false"></button>
          </div>

          <div class="modal-body">
            <div v-if="carrito.length === 0" class="text-center text-muted">
              Tu carrito está vacío 🛒
            </div>

            <table v-else class="table">
              <thead>
                <tr>
                  <th>Producto</th>
                  <th>Cantidad</th>
                  <th>Precio</th>
                  <th>Total</th>
                  <th></th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in carrito" :key="item.id">
                  <td>{{ item.nombre }}</td>
                  <td>
                    <span class="badge bg-primary">{{ item.cantidad }}</span>
                  </td>
                  <td>${{ item.precio }}</td>
                  <td>${{ item.cantidad * item.precio }}</td>
                  <td>
                    <button class="btn btn-sm btn-danger" @click="eliminarProducto(item.id)">
                      Quitar
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>

            <h4 class="text-end" v-if="carrito.length > 0">
              Total a pagar: <strong>${{ totalPagar }}</strong>
            </h4>
          </div>

          <div class="modal-footer">
            <button class="btn btn-secondary" @click="mostrarCarrito = false">
              Cerrar
            </button>
            <button
              class="btn btn-success"
              v-if="carrito.length > 0"
              @click="finalizarCompra"
            >
              Finalizar compra
            </button>
          </div>

        </div>
      </div>
    </div>

    <div v-if="mostrarCarrito" class="modal-backdrop fade show"></div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import TarjetaProducto from "./TarjetaProducto.vue";
import DetalleProducto from "./DetalleProducto.vue";


const productos = ref([
  {
    id: 1,
    nombre: "Roku Express",
    precio: 39990,
    imagen: "../src/assets/img/roku_express.jpg",
    stock: 3,
    descripcion: "Convierte tu TV en Smart TV"
  },
  {
    id: 2,
    nombre: "Mini Dron",
    precio: 530990,
    imagen: "../src/assets/img/mini_dron.jpg",
    stock: 10,
    descripcion: "Precisión y gran alcance"
  }
]);

const carrito = ref(JSON.parse(localStorage.getItem("carrito")) || []);

watch(carrito, () => {
  localStorage.setItem("carrito", JSON.stringify(carrito.value));
}, { deep: true });


function agregarProductoCarrito(id) {
  const prod = productos.value.find(p => p.id === id);
  const item = carrito.value.find(p => p.id === id);

  if (item) {
    item.cantidad++;
  } else {
    carrito.value.push({
      id: prod.id,
      nombre: prod.nombre,
      precio: prod.precio,
      cantidad: 1
    });
  }
}

function eliminarProducto(id) {
  carrito.value = carrito.value.filter(item => item.id !== id);
}

function finalizarCompra() {
  alert("¡Compra realizada con éxito!");
  carrito.value = [];
}


const totalCantidad = computed(() =>
  carrito.value.reduce((acc, item) => acc + item.cantidad, 0)
);

const totalPagar = computed(() =>
  carrito.value.reduce((acc, item) => acc + item.precio * item.cantidad, 0)
);


const productoSeleccionado = ref(null);
const mostrarCarrito = ref(false);
</script>

<style>
.bg-fondo {
  background-image: url('../src/assets/img/fondo.jpg'); 
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}
</style>