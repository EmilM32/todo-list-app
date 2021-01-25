<template>
  <svg
    class="h-8 w-8 text-gray-500 hover:text-gray-600 active:text-green-700 place-self-center cursor-pointer"
    fill="none"
    viewBox="0 0 24 24"
    stroke="currentColor"
    @click="$emit('click', $event)"
  >
    <path
      stroke-linecap="round"
      stroke-linejoin="round"
      stroke-width="2"
      d="M12 9v3m0 0v3m0-3h3m-3 0H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z"
    />
  </svg>
</template>
