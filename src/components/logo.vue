<script setup>
defineProps({
  tamanho: { type: Number, default: 48 },
})
</script>

<template>
  <svg
    :width="tamanho"
    :height="tamanho"
    viewBox="0 0 48 48"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    role="img"
    aria-label="Logo da barbearia"
  >
    <defs>
      <clipPath id="poste">
        <rect x="18" y="10" width="12" height="28" rx="2" />
      </clipPath>
    </defs>

    <!-- tampa de cima e de baixo -->
    <rect x="16" y="4" width="16" height="6" rx="2" fill="currentColor" />
    <rect x="16" y="38" width="16" height="6" rx="2" fill="currentColor" />

    <!-- listras do poste -->
    <g clip-path="url(#poste)" stroke="currentColor" stroke-width="3.5">
      <line x1="14" y1="16" x2="34" y2="8" />
      <line x1="14" y1="24" x2="34" y2="16" />
      <line x1="14" y1="32" x2="34" y2="24" />
      <line x1="14" y1="40" x2="34" y2="32" />
    </g>

    <!-- contorno do poste -->
    <rect x="18" y="10" width="12" height="28" rx="2" stroke="currentColor" stroke-width="1.5" />
  </svg>
</template>