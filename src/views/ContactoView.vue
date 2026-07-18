<script setup>
import { ref, computed } from 'vue'

// 1. Estados reactivos para los campos del formulario
const nombre = ref('')
const edad = ref('')
const correo = ref('')
const celular = ref('')
const sugerencia = ref('')

// Estado para controlar si el formulario ya fue enviado con éxito
const enviadoExitoso = ref(false)

// 2. Propiedades computadas para validar cada campo en tiempo real
const errorNombre = computed(() => {
  if (!nombre.value) return ''
  if (nombre.value.length < 3) return 'El nombre debe tener al menos 3 caracteres.'
  // Expresión regular que solo permite letras y espacios (no números ni símbolos)
  const regexLetras = /^[a-zA-ZáéíóúÁÉÍÓÚñÑ ]+$/
  if (!regexLetras.test(nombre.value)) return 'El nombre no debe contener números ni símbolos.'
  return ''
})

const errorEdad = computed(() => {
  if (!edad.value) return ''
  const valorEdad = parseInt(edad.value)
  if (isNaN(valorEdad) || valorEdad < 18) return 'Debes ser mayor de 18 años.'
  if (valorEdad > 100) return 'Por favor, ingresa una edad válida.'
  return ''
})

const errorCorreo = computed(() => {
  if (!correo.value) return ''
  // Expresión regular estándar para validar correos electrónicos
  const regexCorreo = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!regexCorreo.test(correo.value)) return 'Ingresa un correo electrónico válido (ejemplo@dominio.com).'
  return ''
})

const errorCelular = computed(() => {
  if (!celular.value) return ''
  // Valida que sean exactamente 8 números (estándar en Bolivia)
  const regexCelular = /^[0-9]{8}$/
  if (!regexCelular.test(celular.value)) return 'El celular debe tener exactamente 8 números.'
  return ''
})

// 3. Validar si todo el formulario es completamente válido para habilitar el botón
const formularioValido = computed(() => {
  return nombre.value.length >= 3 && errorNombre.value === '' &&
         edad.value !== '' && errorEdad.value === '' &&
         correo.value !== '' && errorCorreo.value === '' &&
         celular.value !== '' && errorCelular.value === '' &&
         sugerencia.value.trim().length > 0
})

// 4. Función para procesar el envío
const enviarFormulario = () => {
  if (formularioValido.value) {
    enviadoExitoso.value = true
  }
}

// Función para restablecer los campos
const limpiarFormulario = () => {
  nombre.value = ''
  edad.value = ''
  correo.value = ''
  celular.value = ''
  sugerencia.value = ''
  enviadoExitoso.value = false
}
</script>

<template>
  <div class="max-w-5xl mx-auto space-y-8 animate-fade-in">
    <!-- ENCABEZADO -->
    <div class="text-center max-w-xl mx-auto space-y-2">
      <h2 class="text-3xl md:text-4xl font-extrabold text-amber-950 font-serif">Contáctanos</h2>
      <p class="text-stone-600 text-sm md:text-base">
        Tu opinión es muy importante para nosotros. Déjanos tus sugerencias o consultas.
      </p>
      <div class="h-1 w-16 bg-amber-600 mx-auto rounded-full"></div>
    </div>

    <!-- CONTENEDOR PRINCIPAL: Formulario + Información de contacto -->
    <div class="grid grid-cols-1 md:grid-cols-12 gap-8 items-start">
      
      <!-- COLUMNA IZQUIERDA: FORMULARIO CON VALIDACIONES (7 de 12 columnas) -->
      <div class="md:col-span-7 bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-stone-200">
        
        <!-- PANTALLA DE ÉXITO -->
        <div v-if="enviadoExitoso" class="text-center space-y-4 py-6 animate-fade-in">
          <div class="w-16 h-16 bg-emerald-100 text-emerald-800 text-3xl flex items-center justify-center rounded-full mx-auto shadow-sm">
            ✓
          </div>
          <div class="space-y-1">
            <h3 class="text-xl font-bold text-stone-900 font-serif">¡Mensaje enviado con éxito!</h3>
            <p class="text-stone-500 text-sm max-w-xs mx-auto">
              Gracias por tus sugerencias. Nos comunicaremos contigo muy pronto.
            </p>
          </div>
          <button 
            @click="limpiarFormulario" 
            class="bg-amber-950 hover:bg-amber-900 text-amber-100 text-xs font-bold px-5 py-2.5 rounded-xl transition-colors">
            Enviar otro mensaje
          </button>
        </div>

        <!-- EL FORMULARIO -->
        <form v-else @submit.prevent="enviarFormulario" class="space-y-4">
          
          <!-- Nombre -->
          <div class="flex flex-col space-y-1">
            <label class="text-xs font-bold text-stone-700 uppercase tracking-wide">Nombre Completo</label>
            <input 
              v-model="nombre" 
              type="text" 
              placeholder="Ej. Alfredo Flores" 
              class="w-full bg-stone-50 border border-stone-200 rounded-xl px-4 py-2 text-sm text-stone-800 focus:outline-none focus:border-amber-600 transition-colors"
            />
            <p v-if="errorNombre" class="text-xs text-red-600 font-medium pl-1">{{ errorNombre }}</p>
          </div>

          <!-- Fila Doble: Edad y Celular -->
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <!-- Edad -->
            <div class="flex flex-col space-y-1">
              <label class="text-xs font-bold text-stone-700 uppercase tracking-wide">Edad</label>
              <input 
                v-model="edad" 
                type="number" 
                placeholder="Ej. 20" 
                class="w-full bg-stone-50 border border-stone-200 rounded-xl px-4 py-2 text-sm text-stone-800 focus:outline-none focus:border-amber-600 transition-colors"
              />
              <p v-if="errorEdad" class="text-xs text-red-600 font-medium pl-1">{{ errorEdad }}</p>
            </div>

            <!-- Celular -->
            <div class="flex flex-col space-y-1">
              <label class="text-xs font-bold text-stone-700 uppercase tracking-wide">Celular / Teléfono</label>
              <input 
                v-model="celular" 
                type="text" 
                placeholder="Ej. 71234567" 
                class="w-full bg-stone-50 border border-stone-200 rounded-xl px-4 py-2 text-sm text-stone-800 focus:outline-none focus:border-amber-600 transition-colors"
              />
              <p v-if="errorCelular" class="text-xs text-red-600 font-medium pl-1">{{ errorCelular }}</p>
            </div>
          </div>

          <!-- Correo Electrónico -->
          <div class="flex flex-col space-y-1">
            <label class="text-xs font-bold text-stone-700 uppercase tracking-wide">Correo Electrónico</label>
            <input 
              v-model="correo" 
              type="email" 
              placeholder="alfredo@ejemplo.com" 
              class="w-full bg-stone-50 border border-stone-200 rounded-xl px-4 py-2 text-sm text-stone-800 focus:outline-none focus:border-amber-600 transition-colors"
            />
            <p v-if="errorCorreo" class="text-xs text-red-600 font-medium pl-1">{{ errorCorreo }}</p>
          </div>

          <!-- Área de Sugerencias -->
          <div class="flex flex-col space-y-1">
            <label class="text-xs font-bold text-stone-700 uppercase tracking-wide">Buzón de Sugerencias</label>
            <textarea 
              v-model="sugerencia" 
              rows="4" 
              placeholder="Escribe aquí tus comentarios, dudas o sugerencias para mejorar nuestro servicio..." 
              class="w-full bg-stone-50 border border-stone-200 rounded-xl px-4 py-2 text-sm text-stone-800 focus:outline-none focus:border-amber-600 transition-colors resize-none"
            ></textarea>
          </div>

          <!-- Botón de Envío Dinámico -->
          <button 
            type="submit" 
            :disabled="!formularioValido"
            class="w-full font-bold py-3 rounded-xl text-sm transition-all shadow-md mt-2 block text-center text-white
                   disabled:bg-stone-300 disabled:cursor-not-allowed disabled:shadow-none bg-amber-600 hover:bg-amber-700 active:scale-[0.98] transform">
            Enviar Mensaje ✉
          </button>
        </form>

      </div>

      <!-- COLUMNA DERECHA: INFORMACIÓN LATERAL DE LA CAFETERÍA (5 de 12 columnas) -->
      <div class="md:col-span-5 space-y-6">
        <!-- Tarjeta Horarios -->
        <div class="bg-amber-950 text-amber-50 p-6 rounded-3xl shadow-sm space-y-4">
          <h3 class="text-lg font-bold font-serif text-amber-200">Horarios de Atención</h3>
          <div class="space-y-2 text-sm text-stone-200">
            <div class="flex justify-between border-b border-amber-900 pb-1">
              <span>Lunes a Viernes:</span>
              <span class="font-bold text-amber-300">15:30 - 21:00</span>
            </div>
            <div class="flex justify-between border-b border-amber-900 pb-1">
              <span>Sábados:</span>
              <span class="font-bold text-amber-300">16:00 - 22:00</span>
            </div>
            <div class="flex justify-between text-stone-400">
              <span>Domingos:</span>
              <span>Cerrado por descanso</span>
            </div>
          </div>
        </div>

        <!-- Tarjeta Dirección Simulación Ubicación -->
        <div class="bg-white p-6 rounded-3xl shadow-sm border border-stone-200 space-y-3">
          <h3 class="text-lg font-bold font-serif text-amber-950">Nuestra Casa Matriz</h3>
          <p class="text-stone-600 text-sm leading-relaxed">
            📍 Calle Bolívar, entre Petot y Camacho (A media cuadra de la Plaza Principal), Oruro - Bolivia.
          </p>
          <div class="bg-stone-100 text-stone-500 rounded-2xl h-36 flex flex-col items-center justify-center text-center p-4 border border-dashed border-stone-300">
            <span class="text-2xl mb-1">🗺️</span>
            <p class="text-xs font-semibold text-stone-700">Simulación de Mapa GPS activo</p>
            <p class="text-[10px] text-stone-400 mt-0.5">Vista satelital optimizada para la zona central</p>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>