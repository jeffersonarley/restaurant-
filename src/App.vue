<script setup>
import { ref } from 'vue'
import Swal from 'sweetalert2'
import html2pdf from 'html2pdf.js'

// --- 1. BASE DE DATOS (40 PRODUCTOS) ---
const productosBase = [
  // HAMBURGUESAS (1-10)
  { id: 1,  nombre: 'H. Sencilla', precio: 15000, categoria: 'hamburguesas', desc: 'Carne 150g, queso, lechuga y tomate', img: 'https://images.unsplash.com/photo-1568901346375-23c9450c58cd?w=500' },
  { id: 2,  nombre: 'H. Especial', precio: 20000, categoria: 'hamburguesas', desc: 'Carne, queso, tocineta y huevo frito', img: 'https://images.unsplash.com/photo-1594179047519-f347310d3322?w=500' },
  { id: 3,  nombre: 'H. Ranchera', precio: 27000, categoria: 'hamburguesas', desc: 'Carne, salchicha ranchera y BBQ', img: 'https://images.unsplash.com/photo-1551782450-a2132b4ba21d?w=500' },
  { id: 4,  nombre: 'H. Doble', precio: 26000, categoria: 'hamburguesas', desc: 'Doble carne de 150g y doble queso', img: 'https://images.unsplash.com/photo-1606131731446-5568d87113aa?w=500' },
  { id: 5,  nombre: 'H. BBQ', precio: 22000, categoria: 'hamburguesas', desc: 'Salsa BBQ casera y cebolla caramelizada', img: 'https://images.unsplash.com/photo-1550317138-10000687a72b?w=500' },
  { id: 6,  nombre: 'H. Mexicana', precio: 24000, categoria: 'hamburguesas', desc: 'Guacamole, jalapeños y pico de gallo', img: 'https://images.unsplash.com/photo-1596662951482-0c4ba74a6df6?w=500' },
  { id: 7,  nombre: 'H. Criolla', precio: 23000, categoria: 'hamburguesas', desc: 'Hogao, huevo y maduro frito', img: 'https://images.unsplash.com/photo-1512152272829-e3139592d56f?w=500' },
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
  { id: 20, nombre: 'P. Callejero', precio: 8000,  categoria: 'perros', desc: 'El tradicional de carrito', img: 'https://images.unsplash.com/photo-1599599810769-bcde5a160d32?w=500' },
  // ENTRADAS (21-30)
  { id: 21, nombre: 'Salchipapa', precio: 12000, categoria: 'entradas', desc: 'Papas francesas y salchicha manguito', img: 'https://images.unsplash.com/photo-1613010738018-87747e9b0424?w=500' },
  { id: 22, nombre: 'Papas Cheddar', precio: 18000, categoria: 'entradas', desc: 'Queso cheddar y tocineta picada', img: 'https://images.unsplash.com/photo-1585109649139-366815a0d713?w=500' },
  { id: 23, nombre: 'Nuggets x10', precio: 15000, categoria: 'entradas', desc: 'Acompañados de miel-mostaza', img: 'https://images.unsplash.com/photo-1562802378-063ec186a863?w=500' },
  { id: 24, nombre: 'Nachos King', precio: 20000, categoria: 'entradas', desc: 'Con carne desmechada y hogao', img: 'https://images.unsplash.com/photo-1513456852971-30c0b8199d4d?w=500' },
  { id: 25, nombre: 'Alitas BBQ x8', precio: 24000, categoria: 'entradas', desc: 'Bañadas en salsa ahumada', img: 'https://images.unsplash.com/photo-1565557623262-b51c2513a641?w=500' },
  { id: 26, nombre: 'Alitas Hot', precio: 24000, categoria: 'entradas', desc: 'Salsa Búfalo picante', img: 'https://images.unsplash.com/photo-1608139598358-00639965d1b3?w=500' },
  { id: 27, nombre: 'Aros Cebolla', precio: 10000, categoria: 'entradas', desc: 'Crocantes y dorados', img: 'https://images.unsplash.com/photo-1639024471283-03518883511d?w=500' },
  { id: 28, nombre: 'Empanadas x6', precio: 9000,  categoria: 'entradas', desc: 'De carne con ají', img: 'https://images.unsplash.com/photo-1628102424911-3965902f2324?w=500' },
  { id: 29, nombre: 'Patacón Mix', precio: 12000, categoria: 'entradas', desc: 'Con queso y mucho hogao', img: 'https://images.unsplash.com/photo-1599385629703-24ad29202302?w=500' },
  { id: 30, nombre: 'Deditos Queso', precio: 10000, categoria: 'entradas', desc: 'Queso costeño fundido', img: 'https://images.unsplash.com/photo-1534352956274-4b95b3d84a15?w=500' },
  // BEBIDAS (31-40)
  { id: 31, nombre: 'Coca-Cola', precio: 4500,  categoria: 'bebidas', desc: 'Lata 350ml bien fría', img: 'https://images.unsplash.com/photo-1560508180-03f285f67ded?w=500' },
  { id: 32, nombre: 'Limonada Coco', precio: 9500,  categoria: 'bebidas', desc: 'Cremosa y refrescante', img: 'https://images.unsplash.com/photo-1548940740-204726a19be3?w=500' },
  { id: 33, nombre: 'Jugo Fresa', precio: 7000,  categoria: 'bebidas', desc: 'Natural en leche o agua', img: 'https://images.unsplash.com/photo-1563227812-0ea4c22e6cc8?w=500' },
  { id: 34, nombre: 'Agua Manantial', precio: 3000,  categoria: 'bebidas', desc: 'Con o sin gas', img: 'https://images.unsplash.com/photo-1600271886742-f049cd451bba?w=500' },
  { id: 35, nombre: 'Club Colombia', precio: 6000,  categoria: 'bebidas', desc: 'Cerveza Premium', img: 'https://images.unsplash.com/photo-1608270586620-248524c67de9?w=500' },
  { id: 36, nombre: 'Té de Limón', precio: 5000,  categoria: 'bebidas', desc: 'Vaso grande con hielo', img: 'https://images.unsplash.com/photo-1461023058943-07fcbe16d735?w=500' },
  { id: 37, nombre: 'Soda Cereza', precio: 8000,  categoria: 'bebidas', desc: 'Bebida burbujeante artesanal', img: 'https://images.unsplash.com/photo-1571091655789-405eb7a3a3a8?w=500' },
  { id: 38, nombre: 'Malteada Choc', precio: 12000, categoria: 'bebidas', desc: 'Helado cremoso y crema batida', img: 'https://images.unsplash.com/photo-1553361371-9b22f78e8b1d?w=500' },
  { id: 39, nombre: 'Café Frappé', precio: 9000,  categoria: 'bebidas', desc: 'Granizado de café santandereano', img: 'https://images.unsplash.com/photo-1470337458703-46ad1756a187?w=500' },
  { id: 40, nombre: 'Aguapanela', precio: 6000,  categoria: 'bebidas', desc: 'Caliente con queso', img: 'https://images.unsplash.com/photo-1524504388940-b1c1722653e1?w=500' },
]

// --- 2. PERSISTENCIA Y ESTADO ---
const guardados = localStorage.getItem('sabor_real_v2')
const productos = ref(guardados ? JSON.parse(guardados) : productosBase)

const carrito = ref([])
const nombreCliente = ref('')
const activecat = ref('todos')
const mostrarCarrito = ref(false)
const cargando = ref(false)
const pedidoConfirmado = ref(false)
const abrirModal = ref(false)

const nuevoProd = ref({ nombre: '', precio: null, url: '', categoria: 'hamburguesas' })

// --- 3. FUNCIONES ---

function filtrarProductos() {
  if (activecat.value === 'todos') return productos.value
  return productos.value.filter(p => p.categoria === activecat.value)
}

function agregarAlCarrito(p) {
  carrito.value.push({ ...p, idRef: Date.now() + Math.random() })
  Swal.fire({ toast: true, position: 'top-end', icon: 'success', title: 'Agregado', showConfirmButton: false, timer: 800 })
}

function eliminarItem(i) { carrito.value.splice(i, 1) }

function calcularTotal() {
  let t = 0
  for (let item of carrito.value) { t += item.precio }
  return t
}

function procesarPago() {
  if (!nombreCliente.value.trim()) return Swal.fire('Error', 'Ingresa el nombre del cliente', 'warning')
  if (carrito.value.length === 0) return Swal.fire('Vacío', 'Agrega productos al carrito', 'error')

  cargando.value = true
  setTimeout(() => {
    cargando.value = false
    pedidoConfirmado.value = true
    mostrarCarrito.value = false
    Swal.fire('Éxito', 'Pedido generado', 'success')
  }, 1500)
}

function guardarProducto() {
  if (!nuevoProd.value.nombre || !nuevoProd.value.precio) return Swal.fire('Campos obligatorios', '', 'info')
  
  productos.value.push({
    id: Date.now(),
    ...nuevoProd.value,
    desc: 'Producto personalizado'
  })
  
  localStorage.setItem('sabor_real_v2', JSON.stringify(productos.value))
  abrirModal.value = false
  nuevoProd.value = { nombre: '', precio: null, url: '', categoria: 'hamburguesas' }
  Swal.fire('Guardado', 'Datos persistidos en LocalStorage', 'success')
}

function descargarFactura() {
  const element = document.getElementById('factura-pdf')
  const opt = {
    margin: 1,
    filename: `Factura_${nombreCliente.value}.pdf`,
    image: { type: 'jpeg', quality: 0.98 },
    html2canvas: { scale: 3 },
    jsPDF: { unit: 'in', format: 'letter', orientation: 'portrait' }
  }
  html2pdf().set(opt).from(element).save()
}

function reiniciar() {
  pedidoConfirmado.value = false
  carrito.value = []
  nombreCliente.value = ''
}
</script>

<template>
  <div v-if="cargando" class="loader-bg">
    <div class="spin"></div>
    <p>Generando Ticket...</p>
  </div>

  <div class="app-container">
    <button class="fab" @click="abrirModal = true">+</button>

    <header>
      <h1>Sabor Real Express</h1>
      <div class="top-bar">
        <input v-model="nombreCliente" type="text" placeholder="Nombre del Cliente">
        <button class="btn-open-cart" @click="mostrarCarrito = !mostrarCarrito">
          Carrito ({{ carrito.length }})
        </button>
      </div>
    </header>

    <nav class="nav-filters" v-if="!pedidoConfirmado">
      <button v-for="cat in ['todos', 'hamburguesas', 'perros', 'entradas', 'bebidas']"
              :class="{ active: activecat === cat }" @click="activecat = cat">
        {{ cat }}
      </button>
    </nav>

    <main v-if="!pedidoConfirmado" class="product-grid">
      <div class="p-card" v-for="p in filtrarProductos()" :key="p.id">
        <div class="p-img">
          <img :src="p.img || 'https://via.placeholder.com/150'" alt="">
          <span class="badge">{{ p.categoria }}</span>
        </div>
        <div class="p-content">
          <h3>{{ p.nombre }}</h3>
          <p>{{ p.desc }}</p>
          <div class="p-footer">
            <span class="p-price">${{ p.precio.toLocaleString() }}</span>
            <button @click="agregarAlCarrito(p)">+</button>
          </div>
        </div>
      </div>
    </main>

    <section v-else class="receipt-box">
      <div id="factura-pdf" class="ticket">
        <div class="ticket-header">
          <h2>SABOR REAL</h2>
          <p>San Gil, Santander</p>
          <hr>
        </div>
        <div class="ticket-info">
          <p><strong>CLIENTE:</strong> {{ nombreCliente }}</p>
          <p><strong>FECHA:</strong> 25/04/2026</p>
        </div>
        <hr>
        <div class="ticket-list">
          <div v-for="item in carrito" :key="item.idRef" class="t-row">
            <span>{{ item.nombre }}</span>
            <span>${{ item.precio.toLocaleString() }}</span>
          </div>
        </div>
        <hr>
        <div class="t-total">
          <span>TOTAL A PAGAR:</span>
          <strong>${{ calcularTotal().toLocaleString() }}</strong>
        </div>
        <div class="no-pdf-btns">
          <button class="btn-pdf" @click="descargarFactura">📥 Descargar PDF</button>
          <button class="btn-restart" @click="reiniciar">Nueva Venta</button>
        </div>
      </div>
    </section>

    <aside :class="['sidebar', { active: mostrarCarrito }]">
      <div class="side-head">
        <h3>Tu Pedido</h3>
        <button @click="mostrarCarrito = false">×</button>
      </div>
      <div class="side-body">
        <div v-for="(c, i) in carrito" :key="c.idRef" class="c-item">
          <img :src="c.img" width="50" height="50">
          <div class="c-info">
            <p>{{ c.nombre }}</p>
            <span>${{ c.precio.toLocaleString() }}</span>
          </div>
          <button @click="eliminarItem(i)">🗑️</button>
        </div>
      </div>
      <div class="side-foot" v-if="carrito.length > 0">
        <div class="total-bar">
          <span>Total:</span>
          <strong>${{ calcularTotal().toLocaleString() }}</strong>
        </div>
        <button class="btn-pay" @click="procesarPago">Facturar</button>
      </div>
    </aside>

    <div v-if="abrirModal" class="modal">
      <div class="modal-card">
        <h3>Nuevo Producto</h3>
        <input v-model="nuevoProd.nombre" placeholder="Nombre">
        <input v-model.number="nuevoProd.precio" type="number" placeholder="Precio">
        <input v-model="nuevoProd.url" placeholder="URL Imagen">
        <select v-model="nuevoProd.categoria">
          <option value="hamburguesas">Hamburguesas</option>
          <option value="perros">Perros</option>
          <option value="entradas">Entradas</option>
          <option value="bebidas">Bebidas</option>
        </select>
        <div class="m-btns">
          <button @click="abrirModal = false">Cancelar</button>
          <button @click="guardarProducto" class="save-btn">Guardar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.app-container { max-width: 1200px; margin: auto; padding: 20px; font-family: 'Segoe UI', sans-serif; background: #fafafa; min-height: 100vh; }
h1 { text-align: center; font-size: 2.5rem; color: #222; margin-bottom: 20px; }

/* BARRA SUPERIOR */
.top-bar { display: flex; justify-content: center; gap: 10px; margin-bottom: 30px; }
.top-bar input { padding: 12px; border-radius: 8px; border: 1px solid #ddd; width: 300px; }
.btn-open-cart { background: #333; color: white; border: none; padding: 12px 20px; border-radius: 8px; cursor: pointer; }

/* FILTROS */
.nav-filters { display: flex; justify-content: center; gap: 10px; margin-bottom: 30px; }
.nav-filters button { padding: 8px 16px; border-radius: 20px; border: 1px solid #ccc; background: white; cursor: pointer; text-transform: uppercase; font-size: 12px; }
.nav-filters button.active { background: #ff4757; color: white; border-color: #ff4757; }

/* GRID */
.product-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 20px; }
.p-card { background: white; border-radius: 12px; overflow: hidden; box-shadow: 0 4px 6px rgba(0,0,0,0.05); transition: 0.3s; }
.p-card:hover { transform: translateY(-5px); }
.p-img { height: 160px; position: relative; }
.p-img img { width: 100%; height: 100%; object-fit: cover; }
.badge { position: absolute; top: 10px; left: 10px; background: rgba(255,255,255,0.9); padding: 3px 8px; font-size: 10px; border-radius: 5px; color: #ff4757; font-weight: bold; }
.p-content { padding: 15px; }
.p-content h3 { margin: 0; font-size: 18px; }
.p-content p { color: #777; font-size: 13px; margin: 10px 0; min-height: 32px; }
.p-footer { display: flex; justify-content: space-between; align-items: center; }
.p-price { font-weight: bold; font-size: 1.2rem; color: #ff4757; }
.p-footer button { background: #2ed573; color: white; border: none; width: 35px; height: 35px; border-radius: 50%; font-size: 20px; cursor: pointer; }

/* SIDEBAR */
.sidebar { position: fixed; right: -400px; top: 0; width: 350px; height: 100%; background: white; box-shadow: -5px 0 15px rgba(0,0,0,0.1); z-index: 100; transition: 0.4s; display: flex; flex-direction: column; }
.sidebar.active { right: 0; }
.side-head { padding: 20px; border-bottom: 1px solid #eee; display: flex; justify-content: space-between; }
.side-body { flex: 1; overflow-y: auto; padding: 20px; }
.c-item { display: flex; align-items: center; gap: 10px; margin-bottom: 15px; border-bottom: 1px solid #f9f9f9; padding-bottom: 10px; }
.c-item img { border-radius: 5px; }
.c-info { flex: 1; }
.c-info p { margin: 0; font-weight: bold; font-size: 14px; }
.side-foot { padding: 20px; background: #f1f2f6; }
.total-bar { display: flex; justify-content: space-between; margin-bottom: 15px; font-size: 1.2rem; }
.btn-pay { width: 100%; padding: 12px; background: #ff4757; color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; }

/* FACTURA */
.receipt-box { display: flex; justify-content: center; padding: 40px 0; }
.ticket { background: white; padding: 30px; width: 350px; border: 1px solid #ddd; font-family: 'Courier New', Courier, monospace; }
.ticket-header { text-align: center; }
.t-row { display: flex; justify-content: space-between; margin: 8px 0; }
.t-total { display: flex; justify-content: space-between; font-size: 1.2rem; margin-top: 15px; }
.btn-pdf { width: 100%; margin-top: 20px; background: #1e90ff; color: white; border: none; padding: 10px; border-radius: 5px; cursor: pointer; }
.btn-restart { width: 100%; margin-top: 10px; background: #333; color: white; border: none; padding: 10px; border-radius: 5px; cursor: pointer; }

/* FLOATING & MODAL */
.fab { position: fixed; bottom: 20px; right: 20px; width: 60px; height: 60px; border-radius: 50%; background: #2ed573; color: white; border: none; font-size: 30px; cursor: pointer; box-shadow: 0 4px 10px rgba(0,0,0,0.2); }
.modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); display: flex; align-items: center; justify-content: center; z-index: 200; }
.modal-card { background: white; padding: 30px; border-radius: 15px; width: 350px; display: flex; flex-direction: column; gap: 10px; }
.modal-card input, .modal-card select { padding: 10px; border: 1px solid #ddd; border-radius: 5px; }
.save-btn { background: #2ed573; color: white; border: none; padding: 10px; border-radius: 5px; cursor: pointer; }

/* SPINNER */
.loader-bg { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: white; z-index: 500; display: flex; flex-direction: column; align-items: center; justify-content: center; }
.spin { width: 50px; height: 50px; border: 5px solid #f3f3f3; border-top: 5px solid #ff4757; border-radius: 50%; animation: spin 1s linear infinite; }
@keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

/* OCULTAR BOTONES EN PDF */
@media print {
  .no-pdf-btns { display: none; }
}
</style>