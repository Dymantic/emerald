<template>
  <button
    class="absolute md:static center-y right-0 mx-4 text-white text-3xl font-bold next focus:outline-none"
  >
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 32.19 89.36"
      class="h-8 md:h-12 stroke-current text-white"
    >
      <path
        fill="none"
        stroke-miterlimit="10"
        stroke-width="4px"
        d="M1.68 1.08l28.13 43.6L1.68 88.27"
      />
    </svg>
  </button>
</template>

<script>
export default {};
</script>

<style scoped>
.next {
  transition: 0.3s;
}

.next:hover {
  transform: translateY(-50%) scale(1.2);
}
</style>