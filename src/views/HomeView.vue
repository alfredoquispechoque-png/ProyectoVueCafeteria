<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <div class="space-y-12 animate-fade-in">
    <!-- BANNER DE BIENVENIDA (HERO) -->
    <section class="relative bg-amber-950 rounded-3xl overflow-hidden shadow-xl text-amber-50">
      <div class="absolute inset-0 bg-amber-950/90 mix-blend-multiply"></div>
      
      <div class="relative max-w-3xl px-6 py-16 md:py-20 md:px-12 space-y-6 z-10">
        <span class="inline-block bg-amber-700/50 text-amber-300 text-xs font-bold uppercase tracking-widest px-3 py-1 rounded-full border border-amber-500/30">
          Tradición y Sabor desde Oruro
        </span>
        <h2 class="text-3xl md:text-5xl font-extrabold font-serif leading-tight">
          El rincón donde el café se convierte en poesía cálida
        </h2>
        <p class="text-amber-100/80 text-base md:text-lg max-w-xl">
          Disfruta de la perfecta fusión entre granos seleccionados de altura y el toque dulce que reconforta el alma en los días más fríos de nuestra tierra.
        </p>
        <div class="pt-2 flex flex-wrap gap-4">
          <RouterLink to="/productos" class="bg-amber-500 hover:bg-amber-400 text-stone-950 font-bold px-6 py-3 rounded-xl transition-all shadow-md">
            Ver el Menú ☕
          </RouterLink>
          <RouterLink to="/reservas" class="bg-transparent hover:bg-amber-500/20 text-amber-200 border-2 border-amber-500/40 font-semibold px-6 py-3 rounded-xl transition-all">
            Reservar Mesa
          </RouterLink>
        </div>
      </div>
    </section>

    <!-- TARJETAS DE CARACTERÍSTICAS -->
    <section class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <div class="bg-white p-6 rounded-2xl shadow-sm border border-stone-200/60 space-y-2">
        <div class="text-3xl">🏔️</div>
        <h3 class="text-lg font-bold text-amber-950 font-serif">Café de Altura</h3>
        <p class="text-stone-600 text-sm leading-relaxed">
          Granos cultivados en zonas privilegiadas, tostados artesanalmente para resaltar notas intensas y dulces.
        </p>
      </div>

      <div class="bg-white p-6 rounded-2xl shadow-sm border border-stone-200/60 space-y-2">
        <div class="text-3xl">🍰</div>
        <h3 class="text-lg font-bold text-amber-950 font-serif">Pastelería Dulce</h3>
        <p class="text-stone-600 text-sm leading-relaxed">
          Acompañamientos tradicionales y repostería fina horneada diariamente con ingredientes frescos.
        </p>
      </div>

      <div class="bg-white p-6 rounded-2xl shadow-sm border border-stone-200/60 space-y-2">
        <div class="text-3xl">🔥</div>
        <h3 class="text-lg font-bold text-amber-950 font-serif">Ambiente Acogedor</h3>
        <p class="text-stone-600 text-sm leading-relaxed">
          El refugio ideal para estudiar, compartir o disfrutar de un momento al calor de una taza.
        </p>
      </div>
    </section>
  </div>
</template>