<template>
  <div class="min-h-screen bg-gradient-to-br from-slate-50 to-blue-50">
    <!-- Header -->
    

    <main class="max-w-4xl mx-auto px-6 py-8">
      <!-- Title Section -->
      <div class="text-center mb-8">
        <h2 class="text-3xl font-bold text-gray-800 mb-2">Calculadora de Retención</h2>
        <p class="text-lg text-gray-600">5ta Categoría</p>
        <div class="w-24 h-1 bg-sunat-gold mx-auto mt-4 rounded-full"></div>
      </div>

      <!-- Main Calculator Card -->
      <div class="bg-white rounded-2xl shadow-xl overflow-hidden">
        <!-- Worker Data Section -->
        <div class="bg-gradient-to-r from-sunat-blue to-blue-700 px-8 py-6">
          <div class="flex items-center space-x-3">
            <div class="w-8 h-8 bg-white/20 rounded-lg flex items-center justify-center">
              <svg class="w-5 h-5 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path>
              </svg>
            </div>
            <h3 class="text-xl font-semibold text-white">Datos del Trabajador</h3>
          </div>
        </div>

        <div class="p-8">
          <div class="grid md:grid-cols-2 gap-6 mb-8">
            <div class="space-y-2">
              <label class="block text-sm font-medium text-gray-700">Remuneración Mensual (S/)</label>
              <input
                v-model="remuneracion"
                type="number"
                step="0.01"
                class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-sunat-blue focus:border-transparent transition-all duration-200"
                placeholder="0.00"
              />
            </div>
            <div class="space-y-2">
              <label class="block text-sm font-medium text-gray-700">Mes de Inicio del Cálculo</label>
              <select
                v-model="mesInicio"
                class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-sunat-blue focus:border-transparent transition-all duration-200"
              >
                <option v-for="mes in mesesDelAno" :key="mes.valor" :value="mes.valor">{{ mes.nombre }}</option>
              </select>
            </div>
          </div>

          <!-- Previous Retentions -->
          <div class="mb-8">
            <div class="space-y-2">
              <label class="block text-sm font-medium text-gray-700">Retenciones Anteriores (S/)</label>
              <input
                v-model="retencionesAnteriores"
                type="number"
                step="0.01"
                class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-sunat-blue focus:border-transparent transition-all duration-200"
                placeholder="0.00"
              />
            </div>
          </div>

          <!-- Additional Income Section -->
          <div class="border-t pt-8">
            <div class="flex items-center space-x-3 mb-6">
              <div class="w-8 h-8 bg-sunat-gold/20 rounded-lg flex items-center justify-center">
                <svg class="w-5 h-5 text-sunat-gold" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1"></path>
                </svg>
              </div>
              <h3 class="text-xl font-semibold text-gray-800">Ingresos Adicionales (Opcional)</h3>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
              <div class="space-y-2">
                <label class="block text-sm font-medium text-gray-700">Monto (S/)</label>
                <input v-model="ingresoAdicional.monto" type="number" step="0.01" class="w-full px-3 py-2 border border-gray-300 rounded-lg" placeholder="0.00">
              </div>
              <div class="space-y-2">
                <label class="block text-sm font-medium text-gray-700">Mes</label>
                <select v-model="ingresoAdicional.mes" class="w-full px-3 py-2 border border-gray-300 rounded-lg">
                  <option v-for="mes in mesesDelAno" :key="mes.valor" :value="mes.valor">{{ mes.nombre }}</option>
                </select>
              </div>
            </div>

          </div>

          <!-- Calculate Button -->
          <div class="text-center pt-8 mb-8">
            <button
              @click="calcularRetenciones"
              :disabled="!remuneracion || remuneracion <= 0"
              class="bg-gradient-to-r from-sunat-blue to-blue-700 hover:from-blue-700 hover:to-sunat-blue text-white font-semibold py-4 px-8 rounded-xl shadow-lg hover:shadow-xl transform hover:-translate-y-1 transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed disabled:transform-none"
            >
              <span class="flex items-center space-x-2">
                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 7h6m0 10v-3m-3 3h.01M9 17h.01M9 14h.01M12 14h.01M15 11h.01M12 11h.01M9 11h.01M7 21h10a2 2 0 002-2V5a2 2 0 00-2-2H7a2 2 0 00-2 2v14a2 2 0 002 2z"></path>
                </svg>
                <span>Calcular Retenciones</span>
              </span>
            </button>
          </div>

          <!-- Results Section -->
          <div v-if="resultados.mostrar" class="border-t pt-8">
            <div class="flex items-center space-x-3 mb-6">
              <div class="w-8 h-8 bg-green-100 rounded-lg flex items-center justify-center">
                <svg class="w-5 h-5 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v4a2 2 0 01-2 2h-2a2 2 0 01-2-2z"></path>
                </svg>
              </div>
              <h3 class="text-xl font-semibold text-gray-800">Detalle Mensual</h3>
            </div>

            <div class="overflow-x-auto">
              <table class="min-w-full divide-y divide-gray-200">
                <thead class="bg-gray-50">
                  <tr>
                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider\">Mes</th>
                    <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider\">Ingreso Bruto</th>
                    <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider\">Retención</th>
                    <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider\">Ingreso Neto</th>
                  </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-200">
                  <tr v-for="mes in resultados.detallesMensuales" :key="mes.mes">
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">{{ mes.mes }}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500 text-right">S/ {{ formatNumber(mes.ingresoBruto) }}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-red-600 text-right">S/ {{ formatNumber(mes.retencion) }}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-green-600 text-right">S/ {{ formatNumber(mes.ingresoNeto) }}</td>
                  </tr>
                </tbody>
                <tfoot class="bg-gray-100">
                  <tr>
                    <td class="px-6 py-3 text-left text-sm font-bold text-gray-700">TOTAL</td>
                    <td class="px-6 py-3 text-right text-sm font-bold text-gray-700">S/ {{ formatNumber(resultados.totalIngresoBruto) }}</td>
                    <td class="px-6 py-3 text-right text-sm font-bold text-red-700">S/ {{ formatNumber(resultados.totalRetencion) }}</td>
                    <td class="px-6 py-3 text-right text-sm font-bold text-green-700">S/ {{ formatNumber(resultados.totalIngresoNeto) }}</td>
                  </tr>
                </tfoot>
              </table>
            </div>
          </div>

          <!-- Motivational Section -->
          <div v-else class="text-center py-12">
            <div class="w-16 h-16 bg-sunat-gold/20 rounded-full flex items-center justify-center mx-auto mb-4">
              <svg class="w-8 h-8 text-sunat-gold" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
              </svg>
            </div>
            <h3 class="text-2xl font-bold text-gray-800 mb-2">¡Comencemos!</h3>
            <p class="text-gray-600">Complete el formulario y presione calcular para obtener el detalle de sus retenciones</p>
          </div>
        </div>
      </div>

      <!-- Footer Info -->
      <div class="mt-8 text-center text-sm text-gray-500">
        <p>Esta calculadora es referencial. Para información oficial consulte el sitio web de SUNAT.</p>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

// Reactive data
const remuneracion = ref(0)
const mesInicio = ref(1)
const retencionesAnteriores = ref(0)
const ingresoAdicional = reactive({ monto: '', mes: 6 })

const resultados = reactive({
  mostrar: false,
  detallesMensuales: [],
  totalIngresoBruto: 0,
  totalRetencion: 0,
  totalIngresoNeto: 0
})

const mesesDelAno = [
  { valor: 1, nombre: 'Enero' },
  { valor: 2, nombre: 'Febrero' },
  { valor: 3, nombre: 'Marzo' },
  { valor: 4, nombre: 'Abril' },
  { valor: 5, nombre: 'Mayo' },
  { valor: 6, nombre: 'Junio' },
  { valor: 7, nombre: 'Julio' },
  { valor: 8, nombre: 'Agosto' },
  { valor: 9, nombre: 'Septiembre' },
  { valor: 10, nombre: 'Octubre' },
  { valor: 11, nombre: 'Noviembre' },
  { valor: 12, nombre: 'Diciembre' }
]

// UIT 2025 (actualizar según corresponda)
const UIT = 5350 // Valor de la UIT para 2025


// Calculate retention function
const calcularRetenciones = () => {
  const remuneracionMensual = parseFloat(remuneracion.value) || 0;
  const inicioMes = parseInt(mesInicio.value);
  
  // 1. Proyección de ingresos anuales
  const remuneracionProyectada = remuneracionMensual * 12;
  const gratificaciones = (remuneracionMensual * 2); // Julio y Diciembre
  const totalIngresosAdicionales = parseFloat(ingresoAdicional.monto) || 0;
  const ingresoBrutoProyectado = remuneracionProyectada + gratificaciones + totalIngresosAdicionales;

  // 2. Cálculo de la renta neta imponible
  const deduccion = 7 * UIT;
  const baseImponible = Math.max(0, ingresoBrutoProyectado - deduccion);

  // 3. Cálculo del impuesto anual
  let impuestoAnual = 0;
  if (baseImponible <= 5 * UIT) {
    impuestoAnual = baseImponible * 0.08;
  } else if (baseImponible <= 20 * UIT) {
    impuestoAnual = (5 * UIT * 0.08) + ((baseImponible - 5 * UIT) * 0.14);
  } else if (baseImponible <= 35 * UIT) {
    impuestoAnual = (5 * UIT * 0.08) + (15 * UIT * 0.14) + ((baseImponible - 20 * UIT) * 0.17);
  } else if (baseImponible <= 45 * UIT) {
    impuestoAnual = (5 * UIT * 0.08) + (15 * UIT * 0.14) + (15 * UIT * 0.17) + ((baseImponible - 35 * UIT) * 0.20);
  } else {
    impuestoAnual = (5 * UIT * 0.08) + (15 * UIT * 0.14) + (15 * UIT * 0.17) + (10 * UIT * 0.20) + ((baseImponible - 45 * UIT) * 0.30);
  }

  // 4. Cálculo de la retención mensual
  const retencionesHechas = parseFloat(retencionesAnteriores.value) || 0;
  const mesesRestantes = 12 - inicioMes + 1;
  
  let detalles = [];
  let retencionAcumulada = retencionesHechas;

  for (let i = 1; i <= 12; i++) {
    const mesActual = mesesDelAno[i - 1];
    let ingresoBrutoMes = remuneracionMensual;
    let retencionMes = 0;

    // Agregar gratificaciones
    if (i === 7 || i === 12) {
      ingresoBrutoMes += remuneracionMensual;
    }

    // Agregar ingreso adicional del mes
    const ingresoAdicionalMes = (parseInt(ingresoAdicional.mes) === i) ? (parseFloat(ingresoAdicional.monto) || 0) : 0;
    ingresoBrutoMes += ingresoAdicionalMes;

    if (i < inicioMes) {
      // Para meses anteriores, solo mostramos el ingreso, la retención ya se cuenta en 'retencionesAnteriores'
      retencionMes = 0; // No se calcula, se asume ya pagada
    } else {
        // Proyección actualizada considerando ingresos del mes actual
        const ingresosAcumuladosHastaMes = detalles.slice(0, i-1).reduce((acc, mes) => acc + mes.ingresoBruto, 0) + ingresoBrutoMes;
        const proyeccionIngresosRestantes = (12 - i) * remuneracionMensual;
        const gratificacionesPendientes = (i < 7 ? remuneracionMensual : 0) + (i < 12 ? remuneracionMensual : 0);
        const ingresosAdicionalesPendientes = (parseInt(ingresoAdicional.mes) > i) ? (parseFloat(ingresoAdicional.monto) || 0) : 0;
        
        const proyeccionTotalActualizada = ingresosAcumuladosHastaMes + proyeccionIngresosRestantes + gratificacionesPendientes + ingresosAdicionalesPendientes;
        const baseImponibleActualizada = Math.max(0, proyeccionTotalActualizada - deduccion);
        
        // Recalcular impuesto anual con proyección actualizada
        let impuestoAnualActualizado = 0;
        if (baseImponibleActualizada <= 5 * UIT) {
          impuestoAnualActualizado = baseImponibleActualizada * 0.08;
        } else if (baseImponibleActualizada <= 20 * UIT) {
          impuestoAnualActualizado = (5 * UIT * 0.08) + ((baseImponibleActualizada - 5 * UIT) * 0.14);
        } else if (baseImponibleActualizada <= 35 * UIT) {
          impuestoAnualActualizado = (5 * UIT * 0.08) + (15 * UIT * 0.14) + ((baseImponibleActualizada - 20 * UIT) * 0.17);
        } else if (baseImponibleActualizada <= 45 * UIT) {
          impuestoAnualActualizado = (5 * UIT * 0.08) + (15 * UIT * 0.14) + (15 * UIT * 0.17) + ((baseImponibleActualizada - 35 * UIT) * 0.20);
        } else {
          impuestoAnualActualizado = (5 * UIT * 0.08) + (15 * UIT * 0.14) + (15 * UIT * 0.17) + (10 * UIT * 0.20) + ((baseImponibleActualizada - 45 * UIT) * 0.30);
        }
        
        const mesesRestantes = 12 - i + 1;
        const impuestoRestante = Math.max(0, impuestoAnualActualizado - retencionAcumulada);
        retencionMes = mesesRestantes > 0 ? impuestoRestante / mesesRestantes : 0;
        retencionAcumulada += retencionMes;
    }

    detalles.push({
      mes: mesActual.nombre,
      ingresoBruto: ingresoBrutoMes,
      retencion: retencionMes,
      ingresoNeto: ingresoBrutoMes - retencionMes
    });
  }
  
  // Ajuste final para cuadrar la retención anual
  const totalRetenido = detalles.reduce((acc, mes) => acc + mes.retencion, 0) + retencionesHechas;
  const diferencia = impuestoAnual - totalRetenido;
  if (diferencia > 0 && mesesRestantes > 0) {
      const ajustePorMes = diferencia / mesesRestantes;
      for (let i = inicioMes -1; i < 12; i++) {
          detalles[i].retencion += ajustePorMes;
          detalles[i].ingresoNeto -= ajustePorMes;
      }
  }


  resultados.detallesMensuales = detalles;
  resultados.totalIngresoBruto = detalles.reduce((acc, mes) => acc + mes.ingresoBruto, 0);
  resultados.totalRetencion = detalles.reduce((acc, mes) => acc + mes.retencion, 0) + retencionesHechas;
  resultados.totalIngresoNeto = resultados.totalIngresoBruto - resultados.totalRetencion;
  resultados.mostrar = true;
}


// Format number function
const formatNumber = (num) => {
  return new Intl.NumberFormat('es-PE', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2
  }).format(num)
}
</script>