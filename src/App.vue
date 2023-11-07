<script setup>
import { ref, reactive, onMounted } from "vue";

const monedas = ref([
  { codigo: "USD", texto: "Dolar de Estados Unidos" },
  { codigo: "MXN", texto: "Peso Mexicano" },
  { codigo: "EUR", texto: "Euro" },
  { codigo: "GBP", texto: "Libra Esterlina" },
]);

const criptomonedas = ref([]);

const cotizar = reactive({
  moneda: "",
  criptomoneda: "",
});

//Consultar la API
onMounted(() => {
  const url =
    "https://min-api.cryptocompare.com/data/top/mktcapfull?limit=20&tsym=USD";
  fetch(url)
    .then((respuesta) => respuesta.json())
    .then(({ Data }) => (criptomonedas.value = Data));
});
</script>

<template>
  <div class="contenedor">
    <h1 class="titulo">Crypto<span> Check</span></h1>
    <p>
      Cotiza en tiempo real el precio de las
      <span>20 Criptomonedas con más valor</span> o capitalización en el mercado
    </p>

    <div class="contenido">
      <form class="formulario">
        <div class="campo">
          <label for="moneda">Elige tu moneda</label>
          <select id="moneda" v-model="cotizar.moneda">
            <option value="">Selecciona</option>
            <option v-for="moneda in monedas" :value="moneda.codigo">
              {{ moneda.texto }}
            </option>
          </select>
        </div>
        <div class="campo">
          <label for="cripto">Elige tu criptomoneda</label>
          <select id="cripto" v-model="cotizar.criptomoneda">
            <option value="">Selecciona</option>
            <option
              v-for="cripto in criptomonedas"
              :value="cripto.CoinInfo.Name"
            >
              {{ cripto.CoinInfo.FullName }}
            </option>
          </select>
        </div>

        <input type="submit" value="Cotizar" />
      </form>
    </div>
  </div>
</template>

<style>
p {
  color: rgba(255, 255, 255, 0.537);
  line-height: 140%;
  padding-left: 2rem;
}
p span {
  font-weight: 500;
  color: white;
}

@media (min-width: 768px) {
  p span {
    display: block;
  }
}
</style>
