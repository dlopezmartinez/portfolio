<template>
  <svg viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
    <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
    <g
      id="SVGRepo_tracerCarrier"
      stroke-linecap="round"
      stroke-linejoin="round"
    ></g>
    <g id="SVGRepo_iconCarrier">
      <path d="M6 28L4 3H28L26 28L16 31L6 28Z" fill="#E44D26"></path>
      <path d="M26 5H16V29.5L24 27L26 5Z" fill="#F16529"></path>
      <path
        d="M9.5 17.5L8.5 8H24L23.5 11H11.5L12 14.5H23L22 24L16 26L10 24L9.5 19H12.5L13 21.5L16 22.5L19 21.5L19.5 17.5H9.5Z"
        fill="white"
      ></path>
    </g>
  </svg>
</template>

<style scoped>
@keyframes glow {
  0% {
    -webkit-filter: drop-shadow(0px 0px 5px transparent);
    filter: drop-shadow(0px 0px 5px transparent);
  }
  50% {
    -webkit-filter: drop-shadow(0px 0px 10px rgba(227, 76, 38, 0.8));
    filter: drop-shadow(0px 0px 10px rgba(227, 76, 38, 0.8));
  }
  100% {
    -webkit-filter: drop-shadow(0px 0px 5px transparent);
    filter: drop-shadow(0px 0px 5px transparent);
  }
}

svg {
  width: 100%;
  height: 100%;
  /* animation: glow 5s infinite ease-in-out; */
  -webkit-filter: drop-shadow(0px 0px 10px rgba(227, 76, 38, 0.8));
  filter: drop-shadow(0px 0px 10px rgba(227, 76, 38, 0.8));
}
</style>
