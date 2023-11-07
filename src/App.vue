<script setup>
import { ref, reactive, onMounted, computed } from "vue";
import Alerta from "./components/Alerta.vue";

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

const error = ref("");

const cotizacion = ref({});

//Consultar la API
onMounted(() => {
  const url =
    "https://min-api.cryptocompare.com/data/top/mktcapfull?limit=20&tsym=USD";
  fetch(url)
    .then((respuesta) => respuesta.json())
    .then(({ Data }) => (criptomonedas.value = Data));
});

const cotizarCripto = () => {
  // Validar Formulario
  if (Object.values(cotizar).includes("")) {
    error.value = "Todos los campos son obligatorios";
    return;
  }

  error.value = "";
  obtenerCotizacion();
};

const obtenerCotizacion = async () => {
  const { moneda, criptomoneda } = cotizar;
  const url = `https://min-api.cryptocompare.com/data/pricemultifull?fsyms=${criptomoneda}&tsyms=${moneda}`;

  const respuesta = await fetch(url);
  const data = await respuesta.json();

  cotizacion.value = data.DISPLAY[criptomoneda][moneda];
};

const mostrarResultado = computed(() => {
  return Object.values(cotizacion.value).length;
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
      <form class="formulario" @submit.prevent="cotizarCripto">
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

        <Alerta v-if="error">{{ error }}</Alerta>

        <input type="submit" value="Cotizar" />
      </form>

      <div class="contenedor-resultado" v-if="mostrarResultado">
        <h2>Cotización</h2>

        <div class="resultado">
          <img
            :src="'https:/cryptocompare.com/' + cotizacion.IMAGEURL"
            alt="Imagen de la cripto"
          />
          <div>
            <p>
              Su precio es de: <span>{{ cotizacion.PRICE }}</span>
            </p>
            <p>
              Precio más alto del día: <span>{{ cotizacion.HIGHDAY }}</span>
            </p>
            <p>
              Precio más bajo del día: <span>{{ cotizacion.LOWDAY }}</span>
            </p>
            <p>
              Variación ultimas 24 hrs:
              <span>{{ cotizacion.CHANGEPCT24HOUR }}%</span>
            </p>
            <p>
              Última actualización: <span>{{ cotizacion.LASTUPDATE }}</span>
            </p>
          </div>
        </div>
      </div>
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
