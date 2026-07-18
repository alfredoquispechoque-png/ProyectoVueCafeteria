<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div class="min-h-screen bg-amber-50 text-stone-800 flex flex-col font-sans">
    <!-- BARRA DE NAVEGACIÓN -->
    <header class="bg-amber-900 text-amber-50 shadow-md sticky top-0 z-50">
      <div class="max-w-6xl mx-auto px-4 py-4 flex flex-col sm:flex-row justify-between items-center gap-4">
        <!-- Logo / Nombre Local -->
        <h1 class="text-xl md:text-2xl font-bold tracking-wide flex items-center gap-2 text-center sm:text-left">
          ☕ <span class="font-serif italic text-amber-200">Café Dulce Aroma Orureño</span>
        </h1>
        
        <!-- Menú de Enlaces (Las 5 Páginas) -->
<!-- Menú de Enlaces (Las 5 Páginas) -->
        <nav class="flex flex-wrap justify-center gap-4 md:gap-6 font-medium">
          <RouterLink to="/" class="hover:text-amber-300 transition-colors">Inicio</RouterLink>
          <RouterLink to="/about" class="hover:text-amber-300 transition-colors">Nosotros</RouterLink>
          <RouterLink to="/productos" class="hover:text-amber-300 transition-colors">Productos</RouterLink>
          <RouterLink to="/reservas" class="hover:text-amber-300 transition-colors">Reservas</RouterLink>
          <RouterLink to="/contacto" class="hover:text-amber-300 transition-colors">Contacto</RouterLink>
        </nav>
      </div>
    </header>

    <!-- CONTENIDO CAMBIANTE DE LAS PÁGINAS -->
    <main class="flex-1 max-w-6xl w-full mx-auto px-4 py-8">
      <RouterView />
    </main>

    <!-- PIE DE PÁGINA -->
    <footer class="bg-stone-900 text-stone-400 text-center py-4 text-sm border-t border-stone-800">
      <p>&copy; 2026 Café Dulce Aroma Orureño - Proyecto de Programación Web 2</p>
    </footer>
  </div>
</template>