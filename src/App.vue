<script setup>
import  Header from './components/Header.vue';
import  Buttom from './components/buttom.vue';
import {calcular, calcularMesualidad, formatearDinero} from './helpers/index.js'
import {ref, computed, watch } from 'vue';


const cantidad = ref(0);
const meses = ref(6);
const totalPagar = ref(calcular(cantidad.value, meses.value));
const mensualidad = ref(calcularMesualidad(totalPagar.value, meses.value));
const MIN = 0;
const MAX = 20000;
const STEP = 100;

watch([meses, cantidad],() =>{
  totalPagar.value = calcular(cantidad.value, meses.value);
  mensualidad.value = calcularMesualidad(totalPagar.value, meses.value);
});

  const handleButtomRestar =  () => {
    if (cantidad.value > MIN) {
      cantidad.value = cantidad.value - STEP
    }
  }

  const handleButtomSumar =  () => {
    if (cantidad.value < MAX) {
      cantidad.value = cantidad.value + STEP
    }
  }
</script>

<template>
  <div class="my-20 max-w-lg mx-auto bg-white shadow p-10">
    <Header/>

    <div class="flex justify-between mt-10">
     <Buttom :operador="'-'" @fn="handleButtomRestar"/>
     <Buttom :operador="'+'" @fn="handleButtomSumar"/>
    </div>

    <div class="my-5">
        <input 
        type="range"
        class="w-full bg-gray-200 accent-lime-500 hover:accent-lime-600"
        :max="MAX"
        :min="MIN"
        :step="STEP"
        v-model.number="cantidad"
        >
    </div>
    
    <p class="text-center my-10 text-5xl font-extrabold text-indigo-500">{{formatearDinero(cantidad)}}</p>
  
    <h2 class="text-2l font-extrabold text-gray-500 text-center">Elige un <span class="text-indigo-600">Plazo</span></h2>
  
    <select 
     class="w-full p-2 bg-white border border-gray-500 rounded-lg text-center text-xl font-bold text-gray-500 mt-10"
     :value="meses"
     v-model.number="meses" 
    >
      <option value="6">6 Meses</option>
      <option value="12">12 Meses</option>
      <option value="24">24 Meses</option>
    </select>

    <div class="my-5 space-y-3 bg-gray-50 p-5">
        <h2 class="text-2xl font-extrabold text-gray-500 text-center">
          Resumen <span class="text-indigo-600">de pagos</span>
        </h2>
        <p class="text-xl text-gray-500 text-center font-bold">Meses: {{meses}} </p>
        <p class="text-xl text-gray-500 text-center font-bold">Total a pagar:{{formatearDinero(totalPagar)}} </p>
        <p class="text-xl text-gray-500 text-center font-bold">Mensual:{{formatearDinero(mensualidad)}} </p>
    </div>
  </div>

</template>

