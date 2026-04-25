<script setup>
import { ref } from 'vue'
import Swal from 'sweetalert2'

// --- 1. LOS 40 PRODUCTOS (Array Directo) ---
const productos = ref([
  // HAMBURGUESAS (1-10)
  { id: 1,  nombre: 'H. Sencilla', precio: 15000, categoria: 'hamburguesas', desc: 'Carne 150g, queso, lechuga y tomate', img: 'https://images.unsplash.com/photo-1568901346375-23c9450c58cd?w=500' },
  { id: 2,  nombre: 'H. Especial', precio: 20000, categoria: 'hamburguesas', desc: 'Carne, queso, tocineta y huevo frito', img: 'https://images.unsplash.com/photo-1594179047519-f347310d3322?w=500' },
  { id: 3,  nombre: 'H. Ranchera', precio: 27000, categoria: 'hamburguesas', desc: 'Carne, salchicha ranchera y BBQ', img: 'https://images.unsplash.com/photo-1551782450-a2132b4ba21d?w=500' },
  { id: 4,  nombre: 'H. Doble', precio: 26000, categoria: 'hamburguesas', desc: 'Doble carne de 150g y doble queso', img: 'https://images.unsplash.com/photo-1606131731446-5568d87113aa?w=500' },
  { id: 5,  nombre: 'H. BBQ', precio: 22000, categoria: 'hamburguesas', desc: 'Salsa BBQ casera y cebolla caramelizada', img: 'https://images.unsplash.com/photo-1550317138-10000687a72b?w=500' },
  { id: 6,  nombre: 'H. Mexicana', precio: 24000, categoria: 'hamburguesas', desc: 'Guacamole, jalapeños y pico de gallo', img: 'https://images.unsplash.com/photo-1596662951482-0c4ba74a6df6?w=500' },
  { id: 7,  nombre: 'H. Criolla', precio: 23000, categoria: 'hamburguesas', desc: 'Hogar, huevo y maduro frito', img: 'https://images.unsplash.com/photo-1512152272829-e3139592d56f?w=500' },
  { id: 8,  nombre: 'H. Pollo Crispy', precio: 19000, categoria: 'hamburguesas', desc: 'Pechuga apanada y salsa especial', img: 'https://images.unsplash.com/photo-1525059337994-6f2a1311b4d4?w=500' },
  { id: 9,  nombre: 'H. Veggie', precio: 21000, categoria: 'hamburguesas', desc: 'Lentejas, champiñones y rúcula', img: 'https://images.unsplash.com/photo-1520072959219-c595dc870360?w=500' },
  { id: 10, nombre: 'H. Triple Corona', precio: 35000, categoria: 'hamburguesas', desc: 'Tres carnes, extra queso y bacon', img: 'https://images.unsplash.com/photo-1534790566855-4cb788d389ec?w=500' },

  // PERROS (11-20)
  { id: 11, nombre: 'P. Clásico', precio: 10000, categoria: 'perros', desc: 'Salchicha, ripio de papa y salsas', img: 'https://images.unsplash.com/photo-1619740455993-9d622f5d2b83?w=500' },
  { id: 12, nombre: 'P. Chileno', precio: 14000, categoria: 'perros', desc: 'Aguacate picado, tomate y mayo', img: 'https://images.unsplash.com/photo-1563245372-f21724e3856d?w=500' },
  { id: 13, nombre: 'P. Americano', precio: 16000, categoria: 'perros', desc: 'Cebolla picada, mostaza y tocino', img: 'https://images.unsplash.com/photo-1626082927389-6cd097cdc6ec?w=500' },
  { id: 14, nombre: 'P. Suizo', precio: 18000, categoria: 'perros', desc: 'Queso suizo fundido y champiñón', img: 'https://images.unsplash.com/photo-1605663715694-811c750b91e9?w=500' },
  { id: 15, nombre: 'P. Mexicano', precio: 17000, categoria: 'perros', desc: 'Chile con carne y jalapeños', img: 'https://images.unsplash.com/photo-1597733336794-12d05021d510?w=500' },
  { id: 16, nombre: 'Choripán', precio: 15000, categoria: 'perros', desc: 'Chorizo santandereano y chimichurri', img: 'https://images.unsplash.com/photo-1619623861214-5309995be98d?w=500' },
  { id: 17, nombre: 'P. Especial', precio: 22000, categoria: 'perros', desc: 'Salchicha Jumbo y todo el ripio', img: 'https://images.unsplash.com/photo-1541214113241-21578d2d9b62?w=500' },
  { id: 18, nombre: 'P. Hawáiano', precio: 15000, categoria: 'perros', desc: 'Piña melada y jamón', img: 'https://images.unsplash.com/photo-1625938144755-652e08e359b7?w=500' },
  { id: 19, nombre: 'P. Alemán', precio: 20000, categoria: 'perros', desc: 'Bratwurst y chucrut', img: 'https://images.unsplash.com/photo-1532768306203-9b57d0af242e?w=500' },
  { id: 20, nombre: 'P. De la Calle', precio: 8000,  categoria: 'perros', desc: 'El tradicional de carrito', img: 'https://images.unsplash.com/photo-1599599810769-bcde5a160d32?w=500' },

  // ENTRADAS (21-30)
  { id: 21, nombre: 'Salchipapa', precio: 12000, categoria: 'entradas', desc: 'Papas francesas y salchicha manguito', img: 'https://images.unsplash.com/photo-1613010738018-87747e9b0424?w=500' },
  { id: 22, nombre: 'Papas Especiales', precio: 18000, categoria: 'entradas', desc: 'Queso cheddar y tocineta', img: 'https://images.unsplash.com/photo-1585109649139-366815a0d713?w=500' },
  { id: 23, nombre: 'Nuggets x10', precio: 15000, categoria: 'entradas', desc: 'Acompañados de salsa miel-mostaza', img: 'https://images.unsplash.com/photo-1562802378-063ec186a863?w=500' },
  { id: 24, nombre: 'Nachos', precio: 20000, categoria: 'entradas', desc: 'Con carne desmechada y queso', img: 'https://images.unsplash.com/photo-1513456852971-30c0b8199d4d?w=500' },
  { id: 25, nombre: 'Alitas BBQ x8', precio: 24000, categoria: 'entradas', desc: 'Bañadas en salsa dulce y ahumada', img: 'https://images.unsplash.com/photo-1565557623262-b51c2513a641?w=500' },
  { id: 26, nombre: 'Alitas Picantes', precio: 24000, categoria: 'entradas', desc: 'Salsa Búfalo original', img: 'https://images.unsplash.com/photo-1608139598358-00639965d1b3?w=500' },
  { id: 27, nombre: 'Aros de Cebolla', precio: 10000, categoria: 'entradas', desc: 'Crocantes y dorados', img: 'https://images.unsplash.com/photo-1639024471283-03518883511d?w=500' },
  { id: 28, nombre: 'Empanaditas x6', precio: 9000,  categoria: 'entradas', desc: 'De carne con ají casero', img: 'https://images.unsplash.com/photo-1628102424911-3965902f2324?w=500' },
  { id: 29, nombre: 'Patacones', precio: 12000, categoria: 'entradas', desc: 'Con queso y hogao', img: 'https://images.unsplash.com/photo-1599385629703-24ad29202302?w=500' },
  { id: 30, nombre: 'Deditos de Queso', precio: 10000, categoria: 'entradas', desc: 'Queso fundido por dentro', img: 'https://images.unsplash.com/photo-1534352956274-4b95b3d84a15?w=500' },

  // BEBIDAS (31-40)
  { id: 31, nombre: 'Coca-Cola 350ml', precio: 4500,  categoria: 'bebidas', desc: 'Lata bien fría', img: 'https://images.unsplash.com/photo-1560508180-03f285f67ded?w=500' },
  { id: 32, nombre: 'Limonada Coco', precio: 9500,  categoria: 'bebidas', desc: 'Cremosa y refrescante', img: 'https://images.unsplash.com/photo-1548940740-204726a19be3?w=500' },
  { id: 33, nombre: 'Jugo de Fresa', precio: 7000,  categoria: 'bebidas', desc: 'En agua o leche', img: 'https://images.unsplash.com/photo-1563227812-0ea4c22e6cc8?w=500' },
  { id: 34, nombre: 'Agua Mineral', precio: 3000,  categoria: 'bebidas', desc: 'Con o sin gas', img: 'https://images.unsplash.com/photo-1600271886742-f049cd451bba?w=500' },
  { id: 35, nombre: 'Cerveza Club Col.', precio: 6000,  categoria: 'bebidas', desc: 'Pilsen muy fría', img: 'https://images.unsplash.com/photo-1608270586620-248524c67de9?w=500' },
  { id: 36, nombre: 'Té Helado', precio: 5000,  categoria: 'bebidas', desc: 'Sabor Limón', img: 'https://images.unsplash.com/photo-1461023058943-07fcbe16d735?w=500' },
  { id: 37, nombre: 'Soda Cereza', precio: 8000,  categoria: 'bebidas', desc: 'Burbujeante y dulce', img: 'https://images.unsplash.com/photo-1571091655789-405eb7a3a3a8?w=500' },
  { id: 38, nombre: 'Malteada Choc.', precio: 12000, categoria: 'bebidas', desc: 'Helado de chocolate real', img: 'https://images.unsplash.com/photo-1553361371-9b22f78e8b1d?w=500' },
  { id: 39, nombre: 'Granizado Café', precio: 9000,  categoria: 'bebidas', desc: 'Café colombiano frappé', img: 'https://images.unsplash.com/photo-1470337458703-46ad1756a187?w=500' },
  { id: 40, nombre: 'Aguapanela Queso', precio: 6000,  categoria: 'bebidas', desc: 'Tradicional caliente', img: 'https://images.unsplash.com/photo-1524504388940-b1c1722653e1?w=500' },
])

// --- 2. ESTADO DEL SISTEMA ---
const carrito = ref([])
const nombreCliente = ref('')
const activecat = ref('todos')
const mostrarCarrito = ref(false)
const cargando = ref(false)
const pedidoConfirmado = ref(false)
const abrirModal = ref(false)

const nuevoProd = ref({ nombre: '', precio: null, url: '', categoria: 'hamburguesas' })

// --- 3. MÉTODOS (SIN COMPUTED) ---
function filtrarProductos() {
  if (activecat.value === 'todos') return productos.value
  return productos.value.filter(p => p.categoria === activecat.value)
}

function agregarAlCarrito(item) {
  carrito.value.push({ ...item, idCarrito: Date.now() + Math.random() })
  Swal.fire({
    toast: true, position: 'top-end', icon: 'success', title: 'Producto añadido', showConfirmButton: false, timer: 1200
  })
}

function eliminarDelCarrito(index) {
  carrito.value.splice(index, 1)
}

function obtenerTotal() {
  let sum = 0
  for (let p of carrito.value) { sum += p.precio }
  return sum
}

function procesarPedido() {
  if (!nombreCliente.value.trim()) {
    return Swal.fire('Falta el nombre', 'Ingresa el nombre para el ticket', 'warning')
  }
  if (carrito.value.length === 0) {
    return Swal.fire('Carrito vacío', 'Agrega algo para facturar', 'info')
  }

  cargando.value = true // SPINNER UX
  setTimeout(() => {
    cargando.value = false
    pedidoConfirmado.value = true
    mostrarCarrito.value = false
    Swal.fire('¡Éxito!', 'Factura generada', 'success')
  }, 1800)
}

function guardarNuevoProducto() {
  if (!nuevoProd.value.nombre || !nuevoProd.value.precio || !nuevoProd.value.url) {
    return Swal.fire('Campos vacíos', 'Por favor completa todos los datos', 'error')
  }

  productos.value.push({
    id: Date.now(),
    nombre: nuevoProd.value.nombre,
    precio: nuevoProd.value.precio,
    categoria: nuevoProd.value.categoria,
    desc: 'Agregado por el usuario',
    img: nuevoProd.value.url
  })

  abrirModal.value = false
  nuevoProd.value = { nombre: '', precio: null, url: '', categoria: 'hamburguesas' }
  Swal.fire('Guardado', 'El menú ha sido actualizado', 'success')
}
</script>

<template>
  <div v-if="cargando" class="spinner-layer">
    <div class="loader"></div>
    <p>Generando Ticket Fiscal...</p>
  </div>

  <div class="wrap">
    <button class="add-btn-fixed" @click="abrirModal = true" title="Nuevo Producto">+</button>

    <header>
      <h1>Sabor Real Express</h1>
      <p class="loc">ADSO 2026 - San Gil</p>
      
      <div class="user-panel">
        <input type="text" v-model="nombreCliente" placeholder="Cliente: Ej. Juan Pérez">
        <button class="btn-cart-toggle" @click="mostrarCarrito = !mostrarCarrito">
          Ver Orden ({{ carrito.length }})
        </button>
      </div>
    </header>

    <nav class="cat-nav" v-if="!pedidoConfirmado">
      <button v-for="c in ['todos', 'hamburguesas', 'perros', 'entradas', 'bebidas']" 
              :class="{ active: activecat === c }" 
              @click="activecat = c">
        {{ c }}
      </button>
    </nav>

    <main v-if="!pedidoConfirmado" class="products-grid">
      <div class="product-card" v-for="p in filtrarProductos()" :key="p.id">
        <div class="img-wrapper">
          <img :src="p.img" :alt="p.nombre">
          <span class="p-cat">{{ p.categoria }}</span>
        </div>
        <div class="p-body">
          <h3>{{ p.nombre }}</h3>
          <p class="p-desc">{{ p.desc }}</p>
          <div class="p-footer">
            <span class="p-price">${{ p.precio.toLocaleString() }}</span>
            <button @click="agregarAlCarrito(p)">+</button>
          </div>
        </div>
      </div>
    </main>

    <section v-else class="receipt-section">
      <div class="ticket-real">
        <h3>SABOR REAL EXPRESS</h3>
        <p>Nit: 1095-234-1</p>
        <p>San Gil, Santander</p>
        <hr>
        <p><strong>CLIENTE:</strong> {{ nombreCliente.toUpperCase() }}</p>
        <p>FECHA: 18/04/2026</p>
        <hr>
        <div v-for="item in carrito" class="ticket-line">
          <span>{{ item.nombre }}</span>
          <span>${{ item.precio.toLocaleString() }}</span>
        </div>
        <hr>
        <div class="ticket-total">
          <span>TOTAL A PAGAR:</span>
          <strong>${{ obtenerTotal().toLocaleString() }}</strong>
        </div>
        <button @click="pedidoConfirmado = false; carrito = []; nombreCliente = ''" class="btn-new">Nueva Venta</button>
      </div>
    </section>

    <aside :class="['cart-sidebar', { active: mostrarCarrito }]">
      <div class="cart-header">
        <h2>Carrito de Compras</h2>
        <button @click="mostrarCarrito = false">×</button>
      </div>
      <div class="cart-list">
        <div v-for="(p, i) in carrito" class="cart-item">
          <img :src="p.img" width="50">
          <div class="cart-item-info">
            <p>{{ p.nombre }}</p>
            <span>${{ p.precio.toLocaleString() }}</span>
          </div>
          <button @click="eliminarDelCarrito(i)">🗑️</button>
        </div>
      </div>
      <div class="cart-footer" v-if="carrito.length > 0">
        <div class="total-row">
          <span>Total:</span>
          <strong>${{ obtenerTotal().toLocaleString() }}</strong>
        </div>
        <button class="btn-confirm-all" @click="procesarPedido">Confirmar y Facturar</button>
      </div>
    </aside>

    <div v-if="abrirModal" class="modal-overlay">
      <div class="modal-card">
        <h3>Añadir Nuevo Plato</h3>
        <div class="f-group">
          <label>Nombre</label>
          <input v-model="nuevoProd.nombre" type="text">
        </div>
        <div class="f-group">
          <label>Precio</label>
          <input v-model.number="nuevoProd.precio" type="number">
        </div>
        <div class="f-group">
          <label>Imagen (URL)</label>
          <input v-model="nuevoProd.url" type="text" placeholder="https://...">
        </div>
        <div class="f-group">
          <label>Categoría</label>
          <select v-model="nuevoProd.categoria">
            <option value="hamburguesas">Hamburguesa</option>
            <option value="perros">Perro</option>
            <option value="entradas">Entrada</option>
            <option value="bebidas">Bebida</option>
          </select>
        </div>
        <div class="modal-actions">
          <button @click="abrirModal = false" class="btn-cancel">Cancelar</button>
          <button @click="guardarNuevoProducto" class="btn-save">Guardar en Menú</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* GENERAL */
.wrap { max-width: 1100px; margin: auto; padding: 20px; font-family: 'Segoe UI', sans-serif; background: #fdfdfd; min-height: 100vh; position: relative; }
h1 { font-size: 3rem; text-align: center; color: #1a1a1a; margin: 0; }
.loc { text-align: center; color: #777; letter-spacing: 2px; margin-bottom: 2rem; font-size: 14px; }

/* PANEL USUARIO */
.user-panel { display: flex; justify-content: center; gap: 15px; margin-bottom: 30px; }
.user-panel input { padding: 12px 20px; border-radius: 30px; border: 2px solid #eee; width: 300px; outline: none; transition: 0.3s; }
.user-panel input:focus { border-color: #e74c3c; }
.btn-cart-toggle { background: #1a1a1a; color: white; border: none; padding: 12px 30px; border-radius: 30px; cursor: pointer; font-weight: bold; }

/* NAVEGACION */
.cat-nav { display: flex; justify-content: center; gap: 10px; margin-bottom: 30px; }
.cat-nav button { padding: 10px 20px; border-radius: 20px; border: 1px solid #ddd; background: #fff; cursor: pointer; text-transform: uppercase; font-size: 11px; font-weight: 700; }
.cat-nav button.active { background: #e74c3c; color: white; border-color: #e74c3c; box-shadow: 0 4px 10px rgba(231, 76, 60, 0.3); }

/* GRID PRODUCTOS */
.products-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 25px; }
.product-card { background: #fff; border-radius: 15px; overflow: hidden; border: 1px solid #f0f0f0; transition: 0.3s; }
.product-card:hover { transform: translateY(-5px); box-shadow: 0 10px 20px rgba(0,0,0,0.05); }
.img-wrapper { height: 160px; position: relative; }
.img-wrapper img { width: 100%; height: 100%; object-fit: cover; }
.p-cat { position: absolute; top: 10px; left: 10px; background: rgba(255,255,255,0.9); padding: 4px 10px; border-radius: 20px; font-size: 10px; font-weight: 800; text-transform: uppercase; color: #e74c3c; }

.p-body { padding: 15px; }
.p-body h3 { margin: 0 0 8px; font-size: 18px; color: #2c3e50; }
.p-desc { font-size: 13px; color: #888; margin-bottom: 15px; min-height: 32px; }
.p-footer { display: flex; justify-content: space-between; align-items: center; }
.p-price { font-weight: 800; font-size: 20px; color: #e74c3c; }
.p-footer button { background: #2ecc71; color: white; border: none; width: 40px; height: 40px; border-radius: 50%; font-size: 24px; cursor: pointer; }

/* SIDEBAR CARRITO */
.cart-sidebar { position: fixed; right: -400px; top: 0; width: 350px; height: 100%; background: #fff; z-index: 500; transition: 0.4s; box-shadow: -5px 0 15px rgba(0,0,0,0.1); display: flex; flex-direction: column; }
.cart-sidebar.active { right: 0; }
.cart-header { padding: 20px; border-bottom: 1px solid #eee; display: flex; justify-content: space-between; align-items: center; }
.cart-list { flex: 1; overflow-y: auto; padding: 20px; }
.cart-item { display: flex; gap: 15px; align-items: center; margin-bottom: 15px; border-bottom: 1px solid #f9f9f9; padding-bottom: 10px; }
.cart-item-info p { margin: 0; font-weight: bold; font-size: 14px; }
.cart-footer { padding: 20px; background: #f9f9f9; }
.total-row { display: flex; justify-content: space-between; font-size: 1.2rem; margin-bottom: 15px; }
.btn-confirm-all { width: 100%; padding: 15px; background: #e74c3c; color: white; border: none; border-radius: 10px; font-weight: bold; cursor: pointer; }

/* TICKET */
.receipt-section { display: flex; justify-content: center; padding: 40px 0; }
.ticket-real { background: #fff; padding: 30px; border: 1px solid #ddd; width: 300px; box-shadow: 0 5px 15px rgba(0,0,0,0.05); font-family: 'Courier New', Courier, monospace; }
.ticket-real h3 { text-align: center; margin-bottom: 5px; }
.ticket-line { display: flex; justify-content: space-between; font-size: 14px; margin: 5px 0; }
.ticket-total { display: flex; justify-content: space-between; font-size: 18px; margin-top: 15px; }
.btn-new { width: 100%; margin-top: 20px; padding: 10px; background: #1a1a1a; color: white; border: none; cursor: pointer; }

/* MODAL Y SPINNER */
.add-btn-fixed { position: fixed; bottom: 30px; right: 30px; width: 60px; height: 60px; background: #2ecc71; color: white; border: none; border-radius: 50%; font-size: 35px; cursor: pointer; box-shadow: 0 5px 15px rgba(46, 204, 113, 0.4); z-index: 1000; }
.modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); display: flex; justify-content: center; align-items: center; z-index: 1000; }
.modal-card { background: white; padding: 30px; border-radius: 20px; width: 350px; }
.f-group { display: flex; flex-direction: column; margin-bottom: 15px; }
.f-group input, .f-group select { padding: 10px; border-radius: 5px; border: 1px solid #ddd; }
.modal-actions { display: flex; gap: 10px; margin-top: 20px; }
.btn-save { flex: 1; background: #2ecc71; color: white; border: none; padding: 10px; border-radius: 5px; cursor: pointer; font-weight: bold; }
.btn-cancel { flex: 1; background: #eee; border: none; padding: 10px; border-radius: 5px; cursor: pointer; }

.spinner-layer { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: white; z-index: 2000; display: flex; flex-direction: column; align-items: center; justify-content: center; }
.loader { width: 45px; height: 45px; border: 5px solid #f3f3f3; border-top: 5px solid #e74c3c; border-radius: 50%; animation: spin 1s linear infinite; }
@keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
</style>