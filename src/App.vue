<script setup>

import { ref, computed } from 'vue'

const carrito = ref([])

const total = computed(() => {
  return carrito.value.reduce(
    (acumulado, item) =>
      acumulado + item.precio * item.cantidad,
    0
  )
})


const productos = [
  {
    id: 1,
    nombre: "Audífono",
    precio: 30000,
    stock: 3,
    imagen: "/src/assets/images/audifono.jpg"
  },
  {
    id: 2,
    nombre: "Mouse",
    precio: 20000,
    stock: 5,
    imagen: "/src/assets/images/mouse.jpg"
  },
  {
    id: 3,
    nombre: "Teclado",
    precio: 15000,
    stock: 10,
    imagen: "/src/assets/images/teclado.jpg"
  },
  {
    id: 4,
    nombre: "Gabinete",
    precio: 35000,
    stock: 4,
    imagen: "/src/assets/images/gabinete.jpg"
  },
  {
    id: 5,
    nombre: "Pantalla",
    precio: 175000,
    stock: 3,
    imagen: "/src/assets/images/pantalla.jpg"
  },
  {
    id: 6,
    nombre: "Silla",
    precio: 150000,
    stock: 2,
    imagen: "/src/assets/images/silla.jpg"
  }


]

function agregarProducto(producto) {

  if (producto.stock <= 0) {
    alert('No hay más unidades disponibles')
    return
  }

  const item = carrito.value.find(
    p => p.id === producto.id
  )

  if (item) {

    item.cantidad++

  } else {

    carrito.value.push({
      ...producto,
      cantidad: 1
    })

  }

  producto.stock--

}


function removerProducto(id) {

  const item = carrito.value.find(
    producto => producto.id === id
  )

  const productoOriginal = productos.find(
    producto => producto.id === id
  )

  if (item && productoOriginal) {

    productoOriginal.stock += item.cantidad

  }

  carrito.value = carrito.value.filter(
    producto => producto.id !== id
  )

}

</script>

<template>
  <div class="container">
    <h1 class="text-center">
      Carrito de Compras
    </h1>

    <div class="container mt-4">

      <div class="row">

        <div class="col-md-8">

          <div class="row">

            <div v-for="producto in productos" :key="producto.id" class="col-md-4 mb-4">

              <div class="card h-200 shadow">

                <img :src="producto.imagen" :alt="producto.nombre" class="card-img-top">

                <div class="card-body">

                  <h5>{{ producto.nombre }}</h5>

                  <p>
                    Precio:
                    <strong>
                      ${{ producto.precio.toLocaleString('es-CL') }}
                    </strong>
                  </p>

                  <p :class="producto.stock === 0 ? 'text-danger fw-bold' : ''">
                    Stock: {{ producto.stock }}
                  </p>

                  <button class="btn btn-primary" @click="agregarProducto(producto)">
                    Agregar al carrito
                  </button>

                  <!-- 
                  <button class="btn btn-primary" :disabled="producto.stock === 0" @click="agregarProducto(producto)">
                    Agregar al carrito
                  </button>
                  -->

                </div>

              </div>
            </div>
          </div>
        </div>


        <div class="col-md-4">

          <div class="card shadow">

            <div class="card-header">
              <h4>Carrito</h4>
            </div>

            <div class="card-body">

              <div v-if="carrito.length === 0">
                <p>Carrito vacío</p>
              </div>

              <div v-else>

                <div v-for="item in carrito" :key="item.id" class="mb-3 border-bottom pb-2">

                  <p>
                    <img :src="item.imagen" :alt="item.nombre" width="60"> {{ item.nombre }} - Cantidad: {{
                    item.cantidad }}
                  </p>

                  <p>
                    Precio: ${{ item.precio.toLocaleString('es-CL') }}
                  </p>

                  <p>
                    Subtotal: ${{ (item.precio * item.cantidad).toLocaleString('es-CL') }}
                  </p>

                  <button class="btn btn-danger btn-sm" @click="removerProducto(item.id)">
                    Remover del carrito
                  </button>

                </div>

              </div>


            </div>

          </div>

        </div>
        <hr>
        <h1>
          Total:
          ${{ total.toLocaleString('es-CL') }}
        </h1>
      </div>

    </div>


  </div>
</template>


<style>
.card-img-top {
  height: 200px;
  object-fit: contain;
  padding: 10px;
}

.card {
  height: 100%;
}
</style>