<template>
  <svg
    viewBox="0 0 32.00 32.00"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    transform="rotate(0)"
  >
    <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
    <g
      id="SVGRepo_tracerCarrier"
      stroke-linecap="round"
      stroke-linejoin="round"
    ></g>
    <g id="SVGRepo_iconCarrier">
      <path d="M2 4L16 28L30 4H24.5L16 18.5L7.5 4H2Z" fill="#41B883"></path>
      <path
        d="M7.5 4L16 18.5L24.5 4H19.5L16.0653 10.0126L12.5 4H7.5Z"
        fill="#35495E"
      ></path>
    </g>
  </svg>
</template>

<style scoped lang="scss">
@keyframes glow {
  0% {
    -webkit-filter: drop-shadow(0px 0px 5px transparent);
    filter: drop-shadow(0px 0px 5px transparent);
  }
  50% {
    -webkit-filter: drop-shadow(0px 0px 10px rgba(65, 184, 131, 0.8));
    filter: drop-shadow(0px 0px 10px rgba(65, 184, 131, 0.8));
  }
  100% {
    -webkit-filter: drop-shadow(0px 0px 5px transparent);
    filter: drop-shadow(0px 0px 5px transparent);
  }
}

@keyframes rotate {
}

svg {
  width: 100%;
  height: 100%;
  -webkit-filter: drop-shadow(0px 0px 10px rgba(65, 184, 131, 0.8));
  filter: drop-shadow(0px 0px 10px rgba(65, 184, 131, 0.8));
}
</style>
