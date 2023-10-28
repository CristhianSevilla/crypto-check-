<script setup>
import { ref } from "vue";

const monedas = ref([
  { codigo: "USD", texto: "Dolar de Estados Unidos" },
  { codigo: "MXN", texto: "Peso Mexicano" },
  { codigo: "EUR", texto: "Euro" },
  { codigo: "GBP", texto: "Libra Esterlina" },
]);
</script>

<template>
  <div class="contenedor">
    <h1 class="titulo">Crypto<span> Check</span></h1>
    <p>Cotiza Criptomonedas al Instante</p>

    <div class="contenido">
      <form class="formulario">
        <div class="campo">
          <label for="moneda">Elige tu moneda</label>
          <select id="moneda">
            <option value="">Selecciona</option>
            <option v-for="moneda in monedas" :value="moneda.codigo">
              {{ moneda.texto }}
            </option>
          </select>
        </div>
      </form>
    </div>
  </div>
</template>

<style>
p {
  color: white;
}
</style>
