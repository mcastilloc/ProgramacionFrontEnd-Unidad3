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

  const item = carrito.value.find(
    p => p.id === producto.id
  )

  if (item) {

    if (item.cantidad >= producto.stock) {
      alert('No hay más unidades disponibles')
      return
    }

    item.cantidad++

  } else {

    carrito.value.push({
      ...producto,
      cantidad: 1
    })

  }

}


function removerProducto(id) {

  carrito.value = carrito.value.filter(
    item => item.id !== id
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
          <div v-for="producto in productos" :key="producto.id" class="col-md-4 mb-4">

            <div class="card h-100 shadow">

              <img :src="producto.imagen" :alt="producto.nombre" class="card-img-top">

              <div class="card-body">

                <h5>{{ producto.nombre }}</h5>

                <p>
                  Precio:
                  <strong>
                    ${{ producto.precio.toLocaleString('es-CL') }}
                  </strong>
                </p>

                <p>
                  Stock: {{ producto.stock }}
                </p>



                <button class="btn btn-primary" @click="agregarProducto(producto)">
                  Agregar al carrito
                </button>

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

                  <img :src="item.imagen" :alt="item.nombre" width="60">

                  <h6>{{ item.nombre }}</h6>

                  <p>
                    Cantidad: {{ item.cantidad }}
                  </p>

                  <p>
                    Stock: {{ item.stock }}
                  </p>

                  <p>
                    ${{ item.precio.toLocaleString('es-CL') }}
                  </p>

                  <button class="btn btn-danger btn-sm" @click="removerProducto(item.id)">
                    Remover del carrito
                  </button>

                </div>

              </div>

              <hr>

              <h5>
                Total:
                ${{ total.toLocaleString('es-CL') }}
              </h5>

            </div>

          </div>

        </div>

      </div>

    </div>


  </div>
</template>