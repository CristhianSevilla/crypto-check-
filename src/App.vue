<script setup>
import { ref, reactive } from "vue";
import Alerta from "./components/Alerta.vue";
import Spinner from "./components/Spinner.vue";
import useCripto from "./composables/useCrypto";

const {
  monedas,
  criptomonedas,
  cargando,
  cotizacion,
  obtenerCotizacion,
  mostrarResultado,
} = useCripto();

const cotizar = reactive({
  moneda: "",
  criptomoneda: "",
});

const error = ref("");

const cotizarCripto = () => {
  // Validar Formulario
  if (Object.values(cotizar).includes("")) {
    error.value = "Todos los campos son obligatorios";
    return;
  }

  error.value = "";
  obtenerCotizacion(cotizar);
};
</script>

<template>
  <div class="background">
    <div class="contenedor">
      <h1 class="titulo">Crypto<span> Check</span></h1>
      <p class="descripcion">
        Cotiza en tiempo real el precio de las
        <span>20 Criptomonedas con más valor</span> o capitalización en el
        mercado
      </p>

      <div class="contenido">
        <form class="formulario" @submit.prevent="cotizarCripto">
          <div class="campo">
            <label for="moneda">Moneda</label>
            <select id="moneda" v-model="cotizar.moneda">
              <option value="">Selecciona una moneda</option>
              <option v-for="moneda in monedas" :value="moneda.codigo">
                {{ moneda.texto }}
              </option>
            </select>
          </div>
          <div class="campo">
            <label for="cripto">Criptomoneda</label>
            <select id="cripto" v-model="cotizar.criptomoneda">
              <option value="">Selecciona una criptomoneda</option>
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

        <Spinner v-if="cargando" />

        <div class="contenedor-resultado" v-if="mostrarResultado">
          <h2>Cotización</h2>

          <div class="resultado">
            <img
              :src="'https://cryptocompare.com/' + cotizacion.IMAGEURL"
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
  </div>
</template>
