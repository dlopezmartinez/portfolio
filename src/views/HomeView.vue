<script setup>
import { ref } from 'vue'

const helloArr = ['Hola.', 'Hello.', 'Bonjour.', 'Guten tag.', 'Hallo.']
const hello = ref('')

let index = -1
const getNextExpression = () => {
  index = index + 1
  if (index > helloArr.length - 1) {
    index = 0
  }
  return helloArr[index]
}

setInterval(() => {
  const nextValue = getNextExpression()
  console.log('nextValue : ', nextValue)
  hello.value = nextValue
}, 5000)
</script>

<template>
  <header>
    <span>Daniel.dev</span>
  </header>
  <main class="home">
    <article class="home__content">
      <section class="hello__animation--container">
        <transition name="slide-fade">
          <h1 class="hello" :key="hello">
            {{ hello }}
          </h1>
        </transition>
      </section>
    </article>
  </main>
</template>

<style>
.home {
  border: 2px solid red;
  width: 100%;
  height: 100%;
  display: flex;
}

.home__sidebar {
  width: 20%;
  height: 100%;
  background-color: grey;
}

.home__content {
  width: 100%;
  height: 100%;
  background-color: black;
}

.hello__animation--container {
  padding: 100px;
  position: relative;
  display: flex;
  height: 30px;
  align-items: center;
  justify-content: center;
  text-wrap: nowrap;
}

.hello {
  position: absolute;
  display: inline-flex;
}

.slide-fade-enter-active,
.slide-fade-leave-active {
  transition: all 2s ease;
}

.slide-fade-enter-from {
  transform: translateX(-80px);
  opacity: 0;
}

.slide-fade-enter-to {
  transform: translateX(0px);
  opacity: 1;
}

.slide-fade-leave-from {
  transform: translateX(0px);
  opacity: 1;
}

.slide-fade-leave-to {
  transform: translateX(80px);
  opacity: 0;
}
</style>
