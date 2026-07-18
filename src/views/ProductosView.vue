<script setup>
import { ref, computed } from 'vue'

// 1. Estado para saber qué categoría está seleccionada
const categoriaActiva = ref('todos')

// 2. Estado para el carrito de compras
const carrito = ref([])
const mostrarCarrito = ref(false)

// Función para añadir productos al carrito
const agregarAlCarrito = (producto) => {
  // Buscamos si el producto ya existe en el carrito
  const itemExiste = carrito.value.find(item => item.id === producto.id)
  
  if (itemExiste) {
    itemExiste.cantidad++
  } else {
    carrito.value.push({
      id: producto.id,
      nombre: producto.nombre,
      precio: producto.precioNumerico,
      imagen: producto.imagen,
      cantidad: 1
    })
  }
}

// Funciones para modificar cantidades dentro del carrito
const incrementarCantidad = (item) => item.cantidad++
const decrementarCantidad = (item) => {
  item.cantidad--
  if (item.cantidad === 0) {
    carrito.value = carrito.value.filter(i => i.id !== item.id)
  }
}

// 3. Propiedades computadas inteligentes para los totales
const totalProductos = computed(() => {
  return carrito.value.reduce((acc, item) => acc + item.cantidad, 0)
})

const precioTotal = computed(() => {
  return carrito.value.reduce((acc, item) => acc + (item.precio * item.cantidad), 0)
})

// 4. Lista con los 6 productos (añadimos precio numérico para los cálculos)
const productos = ref([
  {
    id: 1,
    nombre: 'Café Expreso Tradicional',
    categoria: 'cafes',
    precioTexto: '12 Bs',
    precioNumerico: 12,
    descripcion: 'Intenso, con una capa de crema perfecta para despertar los sentidos.',
    imagen: '/imagenes/cafe-expreso.jpg'
  },
  {
    id: 2,
    nombre: 'Capuccino con Espuma de Canela',
    categoria: 'cafes',
    precioTexto: '16 Bs',
    precioNumerico: 16,
    descripcion: 'El balance ideal entre espresso, leche evaporada y un toque de canela para combatir el frío.',
    imagen: '/imagenes/cafe-capuccino.jpg'
  },
  {
    id: 3,
    nombre: 'Café de Altura Filtrado',
    categoria: 'cafes',
    precioTexto: '18 Bs',
    precioNumerico: 18,
    descripcion: 'Grano selecto cultivado a gran altura, con notas suaves y maderosas en prensa francesa.',
    imagen: '/imagenes/cafe-filtrado.jpg'
  },
  {
    id: 4,
    nombre: 'Pastel de Chocolate Intenso',
    categoria: 'reposteria',
    precioTexto: '15 Bs',
    precioNumerico: 15,
    descripcion: 'Bizcocho húmedo de chocolate con capas de fudge artesanal.',
    imagen: '/imagenes/postre-chocolate.jpg'
  },
  {
    id: 5,
    nombre: 'Tarta de Tres Leches con Canela',
    categoria: 'reposteria',
    precioTexto: '14 Bs',
    precioNumerico: 14,
    descripcion: 'El postre clásico, tierno y jugoso, con un toque dulce y tradicional.',
    imagen: '/imagenes/postre-tresleches.png'
  },
  {
    id: 6,
    nombre: 'Empanadas de Queso Tradicionales',
    categoria: 'reposteria',
    precioTexto: '8 Bs',
    precioNumerico: 8,
    descripcion: 'Masa crujiente horneada, rellena de abundante queso derretido, ideal para la tarde.',
    imagen: '/imagenes/postre-empanada.png'
  }
])

// Filtrado de categorías
const productosFiltrados = computed(() => {
  if (categoriaActiva.value === 'todos') {
    return productos.value
  }
  return productos.value.filter(p => p.categoria === categoriaActiva.value)
})
</script>

<template>
  <div class="space-y-10 animate-fade-in relative">
    
    <!-- ENCABEZADO Y BOTÓN FLOTANTE DEL CARRITO -->
    <div class="flex justify-between items-center max-w-4xl mx-auto border-b border-stone-200 pb-4">
      <div class="space-y-1">
        <h2 class="text-3xl font-extrabold text-amber-950 font-serif">Nuestro Menú Dulce</h2>
        <p class="text-stone-500 text-sm">Especialidades artesanales para abrigar tus tardes.</p>
      </div>
      
      <!-- Botón del Carrito -->
      <button 
        @click="mostrarCarrito = !mostrarCarrito"
        class="bg-amber-950 hover:bg-amber-900 text-amber-100 px-4 py-3 rounded-2xl flex items-center gap-3 transition-all relative shadow-md">
        <span class="text-xl">🛒</span>
        <span class="font-bold text-sm hidden sm:inline">Mi Carrito</span>
        <span v-if="totalProductos > 0" class="bg-amber-500 text-stone-950 text-xs font-black px-2 py-0.5 rounded-full absolute -top-2 -right-2 animate-bounce">
          {{ totalProductos }}
        </span>
      </button>
    </div>

    <!-- BOTONES DE FILTRO INTERACTIVOS -->
    <div class="flex justify-center gap-3">
      <button 
        @click="categoriaActiva = 'todos'"
        :class="categoriaActiva === 'todos' ? 'bg-amber-600 text-white' : 'bg-stone-100 text-stone-700 hover:bg-stone-200'"
        class="px-5 py-2 rounded-xl text-sm font-medium transition-all shadow-sm">
        ✨ Todos
      </button>
      <button 
        @click="categoriaActiva = 'cafes'"
        :class="categoriaActiva === 'cafes' ? 'bg-amber-600 text-white' : 'bg-stone-100 text-stone-700 hover:bg-stone-200'"
        class="px-5 py-2 rounded-xl text-sm font-medium transition-all shadow-sm">
        ☕ Cafés
      </button>
      <button 
        @click="categoriaActiva = 'reposteria'"
        :class="categoriaActiva === 'reposteria' ? 'bg-amber-600 text-white' : 'bg-stone-100 text-stone-700 hover:bg-stone-200'"
        class="px-5 py-2 rounded-xl text-sm font-medium transition-all shadow-sm">
        🍰 Repostería
      </button>
    </div>

    <!-- GRILLA DE PRODUCTOS -->
    <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
      <div 
        v-for="producto in productosFiltrados" 
        :key="producto.id"
        class="bg-white rounded-2xl overflow-hidden shadow-sm border border-stone-200/70 hover:shadow-md transition-all flex flex-col">
        
        <div class="h-48 bg-stone-100 relative overflow-hidden">
          <img 
            :src="producto.imagen" 
            :alt="producto.nombre"
            class="w-full h-full object-cover"
            @error="$event.target.src = 'https://images.unsplash.com/photo-1509042239860-f550ce710b93?q=80&w=600'" 
          />
          <span class="absolute top-3 right-3 bg-amber-950/90 text-amber-300 font-bold px-3 py-1 rounded-lg text-sm">
            {{ producto.precioTexto }}
          </span>
        </div>

        <div class="p-5 flex-1 flex flex-col justify-between space-y-4">
          <div class="space-y-1">
            <h3 class="font-bold text-lg text-stone-900 font-serif leading-snug">{{ producto.nombre }}</h3>
            <p class="text-stone-500 text-xs md:text-sm leading-relaxed line-clamp-2">{{ producto.descripcion }}</p>
          </div>
          <button 
            @click="agregarAlCarrito(producto)"
            class="w-full bg-stone-900 hover:bg-amber-600 text-amber-50 font-medium py-2.5 rounded-xl text-sm transition-colors shadow-sm active:scale-95 transform">
            ➕ Agregar al carrito
          </button>
        </div>
      </div>
    </section>

    <!-- PANEL LATERAL DEL CARRITO (CARRITO DESPLEGABLE) -->
    <div v-if="mostrarCarrito" class="fixed inset-0 bg-stone-950/40 z-50 flex justify-end animate-fade-in" @click.self="mostrarCarrito = false">
      <div class="bg-white w-full max-w-md h-full shadow-2xl p-6 flex flex-col justify-between overflow-y-auto">
        
        <!-- Cabecera Carrito -->
        <div>
          <div class="flex justify-between items-center border-b border-stone-200 pb-4 mb-4">
            <h3 class="font-bold text-xl text-amber-950 font-serif">Tu Pedido</h3>
            <button @click="mostrarCarrito = false" class="text-stone-400 hover:text-stone-600 text-xl font-bold">✕</button>
          </div>

          <!-- Carrito Vacío -->
          <div v-if="carrito.length === 0" class="text-center py-12 space-y-3">
            <span class="text-5xl">☕</span>
            <p class="text-stone-500 text-sm">Aún no has agregado delicias a tu carrito.</p>
          </div>

          <!-- Items del Carrito -->
          <div v-else class="space-y-4">
            <div v-for="item in carrito" :key="item.id" class="flex items-center gap-4 bg-stone-50 p-3 rounded-xl border border-stone-200/50">
              <img :src="item.imagen" class="w-14 h-14 object-cover rounded-lg" />
              <div class="flex-1">
                <h4 class="font-bold text-sm text-stone-900 leading-tight">{{ item.nombre }}</h4>
                <p class="text-xs text-amber-700 font-medium">{{ item.precio }} Base por unidad</p>
                
                <!-- Controles de cantidad -->
                <div class="flex items-center gap-2 mt-2">
                  <button @click="decrementarCantidad(item)" class="bg-stone-200 hover:bg-stone-300 px-2 py-0.5 rounded text-xs font-bold">-</button>
                  <span class="text-xs font-bold text-stone-800">{{ item.cantidad }}</span>
                  <button @click="incrementarCantidad(item)" class="bg-stone-200 hover:bg-stone-300 px-2 py-0.5 rounded text-xs font-bold">+</button>
                </div>
              </div>
              <div class="text-right font-bold text-sm text-stone-900">
                {{ item.precio * item.cantidad }} Bs
              </div>
            </div>
          </div>
        </div>

        <!-- Total y Confirmación -->
        <div class="border-t border-stone-200 pt-4 mt-6 space-y-4">
          <div class="flex justify-between items-center font-serif text-lg font-bold text-amber-950">
            <span>Total a pagar:</span>
            <span>{{ precioTotal }} Bs</span>
          </div>
          <button 
            :disabled="carrito.length === 0"
            class="w-full bg-amber-600 hover:bg-amber-700 disabled:bg-stone-300 disabled:cursor-not-allowed text-white font-bold py-3 rounded-xl transition-colors shadow-md text-center block">
            Confirmar Pedido ☕
          </button>
        </div>

      </div>
    </div>

  </div>
</template>