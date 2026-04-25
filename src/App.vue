<script setup>
import { ref, computed } from 'vue'
import Swal from 'sweetalert2'

// --- 1. DATOS DE PRODUCTOS ---
const productos = ref([
  { id: 1,  nombre: 'Hamburguesa Sencilla',      precio: 15000, categoria: 'hamburguesas', desc: 'Carne 150g, queso, lechuga y tomate' },
  { id: 2,  nombre: 'Hamburguesa Especial',      precio: 20000, categoria: 'hamburguesas', desc: 'Carne, queso, tocineta y huevo frito' },
  { id: 3,  nombre: 'Hamburguesa Ranchera',      precio: 27000, categoria: 'hamburguesas', desc: 'Carne, salchicha ranchera y BBQ' },
  { id: 4,  nombre: 'Hamburguesa Doble Carne',   precio: 26000, categoria: 'hamburguesas', desc: 'Doble carne de 150g, doble queso' },
  { id: 6,  nombre: 'Hamburguesa Mexicana',      precio: 24000, categoria: 'hamburguesas', desc: 'Carne, guacamole y jalapeños' },
  { id: 7,  nombre: 'Perro Normal',               precio: 10000, categoria: 'perros', desc: 'Salchicha, ripio de papa y salsas' },
  { id: 8,  nombre: 'Perro Chileno',              precio: 14000, categoria: 'perros', desc: 'Salchicha, aguacate picado y tomate' },
  { id: 13, nombre: 'Salchipapa Tradicional',     precio: 12000, categoria: 'entradas', desc: 'Papa francesa y salchicha picada' },
  { id: 15, nombre: 'Mazorcada de Pollo',         precio: 22000, categoria: 'platos', desc: 'Maíz tierno y pollo desmechado' },
  { id: 26, nombre: 'Coca-Cola 350ml',            precio: 4500,  categoria: 'bebidas', desc: 'Lata bien fría' },
  { id: 32, nombre: 'Limonada de Coco',           precio: 9500,  categoria: 'bebidas', desc: 'Refrescante y cremosa' },
  // ... (puedes re-insertar aquí los otros 30 productos siguiendo este formato)
])

// --- 2. MAPA DE IMÁGENES ÚNICAS ---
const imgMap = ref({
  1: 'https://images.unsplash.com/photo-1568901346375-23c9450c58cd?w=500&q=80',
  2: 'https://images.unsplash.com/photo-1594179047519-f347310d3322?w=500&q=80',
  3: 'https://images.unsplash.com/photo-1551782450-a2132b4ba21d?w=500&q=80',
  4: 'https://images.unsplash.com/photo-1606131731446-5568d87113aa?w=500&q=80',
  6: 'https://images.unsplash.com/photo-1596662951482-0c4ba74a6df6?w=500&q=80',
  7: 'https://images.unsplash.com/photo-1619740455993-9d622f5d2b83?w=500&q=80',
  8: 'https://images.unsplash.com/photo-1563245372-f21724e3856d?w=500&q=80',
  13: 'https://images.unsplash.com/photo-1613010738018-87747e9b0424?w=500&q=80',
  15: 'https://images.unsplash.com/photo-1527477396000-e27163b481c2?w=500&q=80',
  26: 'https://images.unsplash.com/photo-1560508180-03f285f67ded?w=500&q=80',
  32: 'https://images.unsplash.com/photo-1548940740-204726a19be3?w=500&q=80',
})

// --- 3. ESTADO DEL SISTEMA ---
const carrito = ref([])
const nombreCliente = ref('')
const activecat = ref('todos')
const mostrarCarrito = ref(false)
const cargando = ref(false)
const pedidoConfirmado = ref(false)
const abrirModal = ref(false)

const nuevoProd = ref({
  nombre: '',
  precio: null,
  url: '',
  categoria: 'hamburguesas'
})

// --- 4. FUNCIONES ---
const itemsFiltrados = computed(() => 
  activecat.value === 'todos' ? productos.value : productos.value.filter(p => p.categoria === activecat.value)
)

function seleccionarProducto(item) {
  carrito.value.push({ ...item, idCarrito: Date.now() + Math.random() })
  Swal.mixin({
    toast: true, position: 'bottom-start', showConfirmButton: false, timer: 1500
  }).fire({ icon: 'success', title: `${item.nombre} añadido` })
}

function eliminarDelCarrito(index) {
  carrito.value.splice(index, 1)
}

function obtenerTotal() {
  return carrito.value.reduce((acc, p) => acc + p.precio, 0)
}

function procesarPedido() {
  if (!nombreCliente.value.trim()) return Swal.fire('Error', 'Ingresa el nombre del cliente', 'error')
  if (carrito.value.length === 0) return Swal.fire('Vacío', 'Agrega productos al carrito', 'warning')
  
  cargando.value = true
  setTimeout(() => {
    cargando.value = false
    pedidoConfirmado.value = true
    mostrarCarrito.value = false
  }, 2000)
}

function agregarProductoNuevo() {
  if (!nuevoProd.value.nombre || !nuevoProd.value.precio || !nuevoProd.value.url) {
    return Swal.fire('Error', 'Completa todos los campos', 'error')
  }

  const idNuevo = Date.now()
  productos.value.push({
    id: idNuevo,
    nombre: nuevoProd.value.nombre,
    precio: nuevoProd.value.precio,
    categoria: nuevoProd.value.categoria,
    desc: 'Nuevo producto del chef'
  })

  imgMap.value[idNuevo] = nuevoProd.value.url
  abrirModal.value = false
  nuevoProd.value = { nombre: '', precio: null, url: '', categoria: 'hamburguesas' }
  
  Swal.fire('¡Éxito!', 'Producto agregado al menú', 'success')
}
</script>

<template>
  <div v-if="cargando" class="spinner-overlay">
    <div class="spinner"></div>
    <p>Cocinando tu pedido...</p>
  </div>

  <div class="wrap">
    <button class="btn-flotante-top" @click="abrirModal = true" title="Nuevo Producto">+</button>

    <header class="main-header">
      <h1>Sabor Real Express</h1>
      <p class="subtitle">Hecho con amor — San Gil, Santander</p>
      
      <div class="user-bar">
        <input type="text" v-model="nombreCliente" placeholder="Nombre del cliente...">
        <button class="cart-toggle" @click="mostrarCarrito = !mostrarCarrito">
          🛒 Pedido ({{ carrito.length }})
        </button>
      </div>
    </header>

    <nav class="cats" v-if="!pedidoConfirmado">
      <button 
        v-for="cat in ['todos', 'hamburguesas', 'perros', 'entradas', 'platos', 'bebidas']" 
        :key="cat"
        class="cat-btn"
        :class="{ active: activecat === cat }"
        @click="activecat = cat"
      >
        {{ cat }}
      </button>
    </nav>

    <main v-if="!pedidoConfirmado" class="grid">
      <div class="card" v-for="item in itemsFiltrados" :key="item.id">
        <div class="img-container">
          <img :src="imgMap[item.id] || 'https://via.placeholder.com/400x300?text=Comida'" :alt="item.nombre">
          <span class="badge" :class="`badge-${item.categoria}`">{{ item.categoria }}</span>
        </div>
        <div class="card-body">
          <h3 class="card-name">{{ item.nombre }}</h3>
          <p class="card-desc">{{ item.desc }}</p>
          <div class="card-row">
            <span class="card-price">${{ item.precio.toLocaleString() }}</span>
            <button @click="seleccionarProducto(item)" class="btn-add">+</button>
          </div>
        </div>
      </div>
    </main>

    <section v-else class="receipt-container">
      <div class="receipt">
        <h2>Sabor Real Express</h2>
        <p>Cliente: <strong>{{ nombreCliente }}</strong></p>
        <hr>
        <div v-for="p in carrito" :key="p.idCarrito" class="receipt-row">
          <span>{{ p.nombre }}</span>
          <span>${{ p.precio.toLocaleString() }}</span>
        </div>
        <hr>
        <div class="receipt-total">
          <span>TOTAL:</span>
          <strong>${{ obtenerTotal().toLocaleString() }}</strong>
        </div>
        <button @click="pedidoConfirmado = false; carrito = []; nombreCliente = ''" class="btn-reset">Nueva Orden</button>
      </div>
    </section>

    <aside :class="['sidebar', { active: mostrarCarrito }]">
      <div class="sidebar-header">
        <h2>Tu Pedido</h2>
        <button @click="mostrarCarrito = false" class="close-btn">×</button>
      </div>
      <div class="sidebar-content">
        <div v-for="(p, i) in carrito" :key="p.idCarrito" class="cart-item">
          <div>
            <p class="item-n">{{ p.nombre }}</p>
            <span class="item-p">${{ p.precio.toLocaleString() }}</span>
          </div>
          <button @click="eliminarDelCarrito(i)">🗑️</button>
        </div>
      </div>
      <div class="sidebar-footer" v-if="carrito.length">
        <div class="total-box">
          <span>Total:</span>
          <span>${{ obtenerTotal().toLocaleString() }}</span>
        </div>
        <button @click="procesarPedido" class="btn-confirm">Confirmar Pedido</button>
      </div>
    </aside>

    <div v-if="abrirModal" class="modal-overlay">
      <div class="modal-content">
        <h3>Nuevo Producto</h3>
        <div class="form-group">
          <label>Nombre del Producto</label>
          <input v-model="nuevoProd.nombre" type="text" placeholder="Ej: Hamburguesa VIP">
        </div>
        <div class="form-group">
          <label>Precio ($)</label>
          <input v-model.number="nuevoProd.precio" type="number" placeholder="Ej: 30000">
        </div>
        <div class="form-group">
          <label>URL de la Imagen</label>
          <input v-model="nuevoProd.url" type="text" placeholder="https://unsplash.com/...">
        </div>
        <div class="form-group">
          <label>Categoría</label>
          <select v-model="nuevoProd.categoria">
            <option value="hamburguesas">Hamburguesas</option>
            <option value="perros">Perros</option>
            <option value="entradas">Entradas</option>
            <option value="platos">Platos</option>
            <option value="bebidas">Bebidas</option>
          </select>
        </div>
        <div class="modal-btns">
          <button class="btn-cancelar" @click="abrirModal = false">Cancelar</button>
          <button class="btn-guardar" @click="agregarProductoNuevo">Guardar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Nunito:wght@400;600;700&display=swap');

.wrap { font-family: 'Nunito', sans-serif; max-width: 1100px; margin: 0 auto; padding: 2rem 1rem; position: relative; }

/* Botón Nuevo Producto arriba a la derecha */
.btn-flotante-top {
  position: absolute;
  top: 20px;
  right: 20px;
  width: 45px;
  height: 45px;
  background: #27ae60;
  color: white;
  border: none;
  border-radius: 50%;
  font-size: 24px;
  cursor: pointer;
  box-shadow: 0 4px 10px rgba(39, 174, 96, 0.3);
  transition: 0.3s;
}
.btn-flotante-top:hover { transform: scale(1.1) rotate(90deg); }

/* Header y Barra de Usuario */
h1 { font-family: 'Playfair Display', serif; text-align: center; font-size: 42px; margin: 0; color: #1a1a1a; }
.subtitle { text-align: center; color: #888; font-size: 12px; letter-spacing: 3px; text-transform: uppercase; margin-bottom: 2rem; }
.user-bar { display: flex; justify-content: center; gap: 15px; margin-bottom: 2rem; }
.user-bar input { padding: 12px 20px; border-radius: 25px; border: 1.5px solid #eee; width: 280px; outline: none; }
.cart-toggle { background: #1a1a1a; color: white; border: none; padding: 12px 25px; border-radius: 25px; cursor: pointer; font-weight: 700; }

/* Categorías */
.cats { display: flex; flex-wrap: wrap; justify-content: center; gap: 10px; margin-bottom: 2.5rem; }
.cat-btn { border: 1.5px solid #eee; background: #fff; color: #666; font-size: 11px; font-weight: 700; padding: 8px 18px; border-radius: 25px; cursor: pointer; text-transform: uppercase; transition: 0.2s; }
.cat-btn.active { background: #c0392b; color: white; border-color: #c0392b; }

/* Cards Grid */
.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 25px; }
.card { background: #fff; border-radius: 18px; overflow: hidden; border: 1px solid #f0f0f0; transition: 0.3s; display: flex; flex-direction: column; }
.card:hover { transform: translateY(-8px); box-shadow: 0 15px 35px rgba(0,0,0,0.07); }
.img-container { position: relative; height: 170px; }
.img-container img { width: 100%; height: 100%; object-fit: cover; }
.badge { position: absolute; top: 12px; left: 12px; font-size: 9px; padding: 5px 12px; border-radius: 20px; font-weight: 800; text-transform: uppercase; }
.badge-hamburguesas { background: #fde8e8; color: #c0392b; }
.badge-perros { background: #fef3e2; color: #b7650a; }
.badge-entradas { background: #e8f5e9; color: #2e7d32; }
.badge-platos { background: #e8eaf6; color: #3949ab; }
.badge-bebidas { background: #e1f5fe; color: #0277bd; }

.card-body { padding: 18px; display: flex; flex-direction: column; flex-grow: 1; }
.card-name { font-size: 17px; font-weight: 700; margin: 0 0 5px; }
.card-desc { font-size: 13px; color: #777; margin-bottom: 15px; flex-grow: 1; }
.card-row { display: flex; justify-content: space-between; align-items: center; }
.card-price { font-size: 19px; font-weight: 800; color: #c0392b; }
.btn-add { background: #27ae60; color: white; border: none; width: 38px; height: 38px; border-radius: 50%; cursor: pointer; font-size: 22px; font-weight: bold; }

/* Sidebar Carrito */
.sidebar { position: fixed; right: -400px; top: 0; width: 350px; height: 100%; background: #fff; z-index: 1000; transition: 0.4s; box-shadow: -10px 0 30px rgba(0,0,0,0.1); display: flex; flex-direction: column; }
.sidebar.active { right: 0; }
.sidebar-header { padding: 25px; border-bottom: 1px solid #eee; display: flex; justify-content: space-between; align-items: center; }
.close-btn { background: none; border: none; font-size: 30px; cursor: pointer; color: #aaa; }
.sidebar-content { flex: 1; overflow-y: auto; padding: 20px; }
.cart-item { display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom: 1px solid #f9f9f9; }
.item-n { font-weight: 700; margin: 0; font-size: 14px; }
.item-p { color: #27ae60; font-size: 13px; font-weight: 600; }
.sidebar-footer { padding: 25px; background: #fafafa; border-top: 1px solid #eee; }
.total-box { display: flex; justify-content: space-between; font-size: 1.3rem; font-weight: 800; margin-bottom: 20px; }
.btn-confirm { width: 100%; padding: 16px; background: #c0392b; color: white; border: none; border-radius: 12px; font-weight: 700; cursor: pointer; }

/* Modal Formulario */
.modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); display: flex; justify-content: center; align-items: center; z-index: 2000; backdrop-filter: blur(3px); }
.modal-content { background: white; padding: 30px; border-radius: 20px; width: 400px; box-shadow: 0 20px 40px rgba(0,0,0,0.3); }
.form-group { margin-bottom: 15px; display: flex; flex-direction: column; }
.form-group label { font-weight: 700; font-size: 13px; margin-bottom: 5px; }
.form-group input, .form-group select { padding: 10px; border: 1px solid #ddd; border-radius: 8px; outline: none; }
.modal-btns { display: flex; gap: 10px; margin-top: 20px; }
.btn-guardar { flex: 1; padding: 12px; background: #27ae60; color: white; border: none; border-radius: 8px; font-weight: 700; cursor: pointer; }
.btn-cancelar { flex: 1; padding: 12px; background: #eee; border: none; border-radius: 8px; cursor: pointer; }

/* Spinner y Factura */
.spinner-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: white; z-index: 3000; display: flex; flex-direction: column; align-items: center; justify-content: center; }
.spinner { width: 50px; height: 50px; border: 5px solid #f3f3f3; border-top: 5px solid #c0392b; border-radius: 50%; animation: spin 1s linear infinite; }
@keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
.receipt-container { display: flex; justify-content: center; padding: 2rem; background: #f9f9f9; border-radius: 20px; }
.receipt { background: white; padding: 30px; border: 2px dashed #bbb; width: 340px; font-family: monospace; }
.receipt h2 { text-align: center; margin-top: 0; }
.receipt-row { display: flex; justify-content: space-between; margin: 8px 0; }
.receipt-total { display: flex; justify-content: space-between; font-size: 1.2rem; font-weight: 800; margin-top: 15px; }
.btn-reset { width: 100%; margin-top: 20px; padding: 12px; background: #1a1a1a; color: white; border: none; border-radius: 8px; cursor: pointer; font-weight: 700; }
</style>