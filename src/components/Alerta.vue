<template>
  <div class="alerta">
    <slot></slot>
  </div>
</template>

<style>
.alerta {
  text-align: center;
  color: var(--blanco);
  background-color: rgba(131, 6, 6, 0.841);
  text-transform: uppercase;
  font-weight: 700;
  font-size: 1.4rem;
  padding: 1rem 0.5rem;
  border-radius: 1rem;
}
</style>
