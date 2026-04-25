 <script setup>
import { ref } from 'vue'

const productos = ref([
  // ... (Tus 40 productos se mantienen igual)
  { id: 1, nombre: 'Hamburguesa Sencilla', precio: 15000, categoria: 'hamburguesas' },
  { id: 2, nombre: 'Hamburguesa Especial', precio: 20000, categoria: 'hamburguesas' },
  { id: 3, nombre: 'Hamburguesa Ranchera', precio: 27000, categoria: 'hamburguesas' },
  { id: 4, nombre: 'Hamburguesa Doble Carne', precio: 26000, categoria: 'hamburguesas' },
  { id: 5, nombre: 'Hamburguesa BBQ', precio: 22000, categoria: 'hamburguesas' },
  { id: 6, nombre: 'Hamburguesa Mexicana', precio: 24000, categoria: 'hamburguesas' },
  { id: 7, nombre: 'Perro Normal', precio: 10000, categoria: 'perros' },
  { id: 8, nombre: 'Perro Chileno', precio: 14000, categoria: 'perros' },
  { id: 9, nombre: 'Perro Americano', precio: 16000, categoria: 'perros' },
  { id: 10, nombre: 'Perro Suizo', precio: 18000, categoria: 'perros' },
  { id: 11, nombre: 'Perro Especial de la Casa', precio: 22000, categoria: 'perros' },
  { id: 12, nombre: 'Mega Perro', precio: 25000, categoria: 'perros' },
  { id: 13, nombre: 'Salchipapa Tradicional', precio: 12000, categoria: 'entradas' },
  { id: 14, nombre: 'Salchipapa Especial', precio: 18000, categoria: 'entradas' },
  { id: 15, nombre: 'Mazorcada de Pollo', precio: 22000, categoria: 'platos' },
  { id: 16, nombre: 'Mazorcada Mixta', precio: 26000, categoria: 'platos' },
  { id: 17, nombre: 'Desgranado de Lomo', precio: 28000, categoria: 'platos' },
  { id: 18, nombre: 'Porción de Papas Fritas', precio: 7000, categoria: 'acompañantes' },
  { id: 19, nombre: 'Nuggets de Pollo x6', precio: 12000, categoria: 'acompañantes' },
  { id: 20, nombre: 'Alitas BBQ x8', precio: 24000, categoria: 'platos' },
  { id: 21, nombre: 'Alitas Picantes x8', precio: 24000, categoria: 'platos' },
  { id: 22, nombre: 'Club Sandwich', precio: 18000, categoria: 'platos' },
  { id: 23, nombre: 'Quesadilla de Pollo', precio: 16000, categoria: 'platos' },
  { id: 24, nombre: 'Nachos con Carne', precio: 20000, categoria: 'entradas' },
  { id: 25, nombre: 'Choripán Argentino', precio: 15000, categoria: 'perros' },
  { id: 26, nombre: 'Coca-Cola 350ml', precio: 4500, categoria: 'bebidas' },
  { id: 27, nombre: 'Coca-Cola 1.5L', precio: 8500, categoria: 'bebidas' },
  { id: 28, nombre: 'Agua Mineral', precio: 3000, categoria: 'bebidas' },
  { id: 29, nombre: 'Jugo Natural en Agua', precio: 6000, categoria: 'bebidas' },
  { id: 30, nombre: 'Jugo Natural en Leche', precio: 7500, categoria: 'bebidas' },
  { id: 31, nombre: 'Limonada Natural', precio: 6500, categoria: 'bebidas' },
  { id: 32, nombre: 'Limonada de Coco', precio: 9500, categoria: 'bebidas' },
  { id: 33, nombre: 'Té Helado', precio: 5000, categoria: 'bebidas' },
  { id: 34, nombre: 'Cerveza Nacional', precio: 6000, categoria: 'bebidas' },
  { id: 35, nombre: 'Cerveza Importada', precio: 10000, categoria: 'bebidas' },
  { id: 36, nombre: 'Malteada de Vainilla', precio: 12000, categoria: 'bebidas' },
  { id: 37, nombre: 'Malteada de Chocolate', precio: 12000, categoria: 'bebidas' },
  { id: 38, nombre: 'Soda Saborizada', precio: 8000, categoria: 'bebidas' },
  { id: 39, nombre: 'Granizado de Café', precio: 9000, categoria: 'bebidas' },
  { id: 40, nombre: 'Agua Panela con Queso', precio: 5500, categoria: 'bebidas' }
])

const carrito = ref([])
const nombreCliente = ref('')

function seleccionarProducto(item) {
  // Creamos una copia con un ID de carrito único para poder borrar items individuales
  carrito.value.push({ ...item, idCarrito: Date.now() + Math.random() })
}

function eliminarDelCarrito(index) {
  carrito.value.splice(index, 1)
}
</script>

<template>
  <div class="menu">
    <header>
      <h1>Fast Food Jefferson</h1>
      <div class="user-input">
        <label>Nombre del cliente: </label>
        <input type="text" v-model="nombreCliente" placeholder="¿A nombre de quién?">
      </div>
    </header>
  </div>

  <main class="contenedor-principal">
    <section class="menu-sections">
      <h2>Carta de Productos</h2>
      <div class="grid-productos">
        <div v-for="item in productos" :key="item.id" class="cajita-producto">
          <h3 class="nombre">{{ item.nombre }}</h3>
          <p class="categoria-tag">{{ item.categoria }}</p>
          <p class="precio">${{ item.precio.toLocaleString() }}</p>
          <button @click="seleccionarProducto(item)" class="btn-agregar">
            agregar
          </button>
        </div>
      </div>
    </section>

    <aside class="carrito-compras">
      <h2>🛒 Pedido de: {{ nombreCliente || 'Cliente' }}</h2>
      <div v-if="carrito.length === 0" class="mensaje-vacio">
        No hay productos en el carrito.
      </div>
      
      <div v-else class="lista-carrito">
        <div v-for="(prod, index) in carrito" :key="prod.idCarrito" class="item-carrito">
          <div class="info-carrito">
            <span class="nombre-c">{{ prod.nombre }}</span>
            <span class="precio-c">${{ prod.precio.toLocaleString() }}</span>
          </div>
          <button @click="eliminarDelCarrito(index)" class="btn-quitar">🗑️</button>
        </div>

        <div class="total">
          <hr>
          <h3>Total: ${{ carrito.reduce((acc, p) => acc + p.precio, 0).toLocaleString() }}</h3>
          <button class="btn-ordenar" @click="alert('Pedido enviado para ' + nombreCliente)">
            Confirmar Orden
          </button>
        </div>
      </div>
    </aside>
  </main>
</template>

<style scoped>
.contenedor-principal {
  display: flex;
  gap: 30px;
  padding: 20px;
}

.menu-sections {
  flex: 2;
}

/* El div distinto para el carrito */
.carrito-compras {
  flex: 1;
  background-color: #f4f4f4;
  padding: 20px;
  border-radius: 15px;
  height: fit-content;
  position: sticky;
  top: 20px;
  border: 1px solid #ddd;
}

.grid-productos {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 15px;
}
.cajita-producto {
.cajita-producto {
  border: 1px solid #ccc;
  background: white;
}

}
.item-carrito {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid #ddd;
}

.btn-agregar {
  background-color: #2ecc71;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
  cursor: pointer;
}

.btn-quitar {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.2rem;
}

.btn-ordenar {
  width: 100%;
  background-color: #e67e22;
  color: white;
  padding: 10px;
  border: none;
  border-radius: 5px;
  margin-top: 10px;
  cursor: pointer;
}
</style>