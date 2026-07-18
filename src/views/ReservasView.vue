<script setup>
import { ref } from 'vue'

// 1. Estados reactivos para los campos del formulario
const nombre = ref('')
const personas = ref('2')
const fecha = ref('')
const hora = ref('')
const notas = ref('')

// Estado para controlar si ya se envió la reserva
const reservaConfirmada = ref(false)

// 2. Función interactiva al enviar el formulario
const procesarReserva = () => {
  reservaConfirmada.value = true
}

// Función para reiniciar el formulario y hacer otra reserva
const nuevaReserva = () => {
  nombre.value = ''
  personas.value = '2'
  fecha.value = ''
  hora.value = ''
  notas.value = ''
  reservaConfirmada.value = false
}
</script>

<template>
  <div class="max-w-5xl mx-auto space-y-8 animate-fade-in">
    <!-- ENCABEZADO -->
    <div class="text-center max-w-xl mx-auto space-y-2">
      <h2 class="text-3xl md:text-4xl font-extrabold text-amber-950 font-serif">Reserva tu Mesa</h2>
      <p class="text-stone-600 text-sm md:text-base">
        Asegura un espacio cálido y acogedor para compartir momentos inolvidables.
      </p>
      <div class="h-1 w-16 bg-amber-600 mx-auto rounded-full"></div>
    </div>

    <!-- CONTENEDOR PRINCIPAL dividido en 2 columnas -->
    <div class="bg-white rounded-3xl shadow-sm border border-stone-200 overflow-hidden grid grid-cols-1 md:grid-cols-12">
      
      <!-- COLUMNA 1: DISEÑO E INFORMACIÓN (5 de 12 columnas) -->
      <div class="md:col-span-5 bg-amber-950 p-8 text-amber-50 flex flex-col justify-between space-y-8">
        <div class="space-y-4">
          <span class="text-xs font-bold uppercase tracking-widest bg-amber-800 text-amber-300 px-3 py-1 rounded-full border border-amber-600 inline-block">
            Experiencia Dulce Aroma
          </span>
          <h3 class="text-2xl font-bold font-serif leading-tight text-amber-100">¿Por qué reservar con nosotros?</h3>
          <p class="text-stone-300 text-sm leading-relaxed">
            Nuestras mesas están distribuidas estratégicamente para ofrecerte privacidad, comodidad y el calor ideal frente al clima de Oruro.
          </p>
        </div>

        <!-- Lista de beneficios visuales -->
        <div class="space-y-3 text-sm">
          <div class="flex items-center gap-3">
            <span class="text-amber-400">🔥</span>
            <p class="text-stone-200">Ambiente climatizado y acogedor.</p>
          </div>
          <div class="flex items-center gap-3">
            <span class="text-amber-400">🔌</span>
            <p class="text-stone-200">Zonas con conectores ideales para estudiar o trabajar.</p>
          </div>
          <div class="flex items-center gap-3">
            <span class="text-amber-400">🎂</span>
            <p class="text-stone-200">Atención especial para cumpleaños y reuniones.</p>
          </div>
        </div>

        <div class="text-xs text-amber-400 border-t border-amber-900 pt-4">
          Nota: Tolerancia máxima de 15 minutos en tu reserva.
        </div>
      </div>

      <!-- COLUMNA 2: FORMULARIO INTERACTIVO (7 de 12 columnas) -->
      <div class="md:col-span-7 p-8 flex flex-col justify-center">
        
        <!-- VISTA A: PANTALLA DE ÉXITO ANIMADA -->
        <div v-if="reservaConfirmada" class="text-center space-y-5 py-8 animate-fade-in">
          <div class="w-16 h-16 bg-amber-100 text-amber-800 text-3xl flex items-center justify-center rounded-full mx-auto shadow-sm">
            ✨
          </div>
          <div class="space-y-2">
            <h3 class="text-2xl font-bold text-amber-950 font-serif">¡Tu mesa está reservada, {{ nombre }}!</h3>
            <p class="text-stone-600 text-sm max-w-sm mx-auto">
              Te esperamos el <span class="font-bold text-stone-800">{{ fecha }}</span> a las <span class="font-bold text-stone-800">{{ hora }}</span> para disfrutar juntos de la mejor experiencia dulce.
            </p>
          </div>
          <button 
            @click="nuevaReserva" 
            class="bg-amber-950 hover:bg-amber-900 text-amber-100 text-sm font-medium px-6 py-2.5 rounded-xl transition-colors shadow-sm">
            Hacer otra reserva
          </button>
        </div>

        <!-- VISTA B: EL FORMULARIO REAL -->
        <form v-else @submit.prevent="procesarReserva" class="space-y-4">
          <!-- Campo Nombre -->
          <div class="flex flex-col space-y-1">
            <label class="text-xs font-bold text-stone-700 uppercase tracking-wide">Nombre Completo</label>
            <input 
              v-model="nombre" 
              type="text" 
              required 
              placeholder="Ej. Alfredo Flores" 
              class="w-full bg-stone-50 border border-stone-200 rounded-xl px-4 py-2.5 text-sm text-stone-800 focus:outline-none focus:border-amber-600 transition-colors"
            />
          </div>

          <!-- Fila Doble: Cantidad de Personas y Hora -->
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div class="flex flex-col space-y-1">
              <label class="text-xs font-bold text-stone-700 uppercase tracking-wide">Nº de Personas</label>
              <select 
                v-model="personas" 
                class="w-full bg-stone-50 border border-stone-200 rounded-xl px-3 py-2.5 text-sm text-stone-800 focus:outline-none focus:border-amber-600 transition-colors">
                <option value="1">1 Persona</option>
                <option value="2">2 Personas</option>
                <option value="3">3 Personas</option>
                <option value="4">4 Personas</option>
                <option value="5+">5 o más personas</option>
              </select>
            </div>

            <div class="flex flex-col space-y-1">
              <label class="text-xs font-bold text-stone-700 uppercase tracking-wide">Hora</label>
              <input 
                v-model="hora" 
                type="time" 
                required 
                class="w-full bg-stone-50 border border-stone-200 rounded-xl px-4 py-2.5 text-sm text-stone-800 focus:outline-none focus:border-amber-600 transition-colors"
              />
            </div>
          </div>

          <!-- Campo Fecha -->
          <div class="flex flex-col space-y-1">
            <label class="text-xs font-bold text-stone-700 uppercase tracking-wide">Fecha</label>
            <input 
              v-model="fecha" 
              type="date" 
              required 
              class="w-full bg-stone-50 border border-stone-200 rounded-xl px-4 py-2.5 text-sm text-stone-800 focus:outline-none focus:border-amber-600 transition-colors"
            />
          </div>

          <!-- Campo Notas Extra -->
          <div class="flex flex-col space-y-1">
            <label class="text-xs font-bold text-stone-700 uppercase tracking-wide">Notas Especiales (Opcional)</label>
            <textarea 
              v-model="notas" 
              rows="2" 
              placeholder="Ej. Mesa cerca de la calefacción, celebración de cumpleaños, etc." 
              class="w-full bg-stone-50 border border-stone-200 rounded-xl px-4 py-2.5 text-sm text-stone-800 focus:outline-none focus:border-amber-600 transition-colors resize-none"
            ></textarea>
          </div>

          <!-- Botón de Envío -->
          <button 
            type="submit" 
            class="w-full bg-amber-600 hover:bg-amber-700 text-white font-bold py-3 rounded-xl text-sm transition-all shadow-md active:scale-[0.98] transform mt-2">
            Confirmar Reserva 📅
          </button>
        </form>

      </div>
    </div>
  </div>
</template>