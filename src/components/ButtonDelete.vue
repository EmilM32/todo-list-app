<template>
  <svg
    class="h-4 w-4 text-red-700 place-self-center cursor-pointer"
    fill="none"
    viewBox="0 0 24 24"
    stroke="currentColor"
    @click="$emit('click', $event)"
  >
    <path
      stroke-linecap="round"
      stroke-linejoin="round"
      stroke-width="2"
      d="M6 18L18 6M6 6l12 12"
    />
  </svg>
</template>
