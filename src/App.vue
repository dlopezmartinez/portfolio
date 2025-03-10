<!-- eslint-disable no-unused-vars -->
<script setup>
import { ref } from "vue";
import LinkednIcon from "./components/icons/LinkednIcon.vue";
import CVIcon from "./components/icons/CVIcon.vue";
import VueIcon from "./components/icons/VueIcon.vue";
import JavascriptIcon from "./components/icons/JavascriptIcon.vue";
import CssIcon from "./components/icons/CSSIcon.vue";
import HtmlIcon from "./components/icons/HTMLIcon.vue";
import ViteIcon from "./components/icons/ViteIcon.vue";
import AngularIcon from "./components/icons/AngularIcon.vue";
import WebpackIcon from "./components/icons/WebpackIcon.vue";
import PiniaIcon from "./components/icons/PiniaIcon.vue";
// -- isMobile --
// detectamos el viewport para eliminar ciertos elementos o animaciones
const isMobile = ref(window.innerWidth <= 899);
window.addEventListener("resize", (event) => {
  isMobile.value = event.target.innerWidth <= 899;
});

// -- hola --

const helloArr = ["Hola,", "Hello,", "Bonjour,", "Guten tag,", "Hallo,"];
const hello = ref("");
const description = ref("");

let index = -1;
const getNextExpression = () => {
  index = index + 1;
  if (index > helloArr.length - 1) {
    index = 0;
  }
  return helloArr[index];
};

setTimeout(() => {
  hello.value = "Hola,";
  description.value = "Soy Daniel, un apasionado desarrollador de software.";
}, 1000);

const intervalTime = 4000;
let interval = setInterval(() => {
  const nextValue = getNextExpression();
  hello.value = nextValue;
}, intervalTime);

document.addEventListener("visibilitychange", function () {
  if (document.hidden) {
    clearInterval(interval);
  } else {
    interval = setInterval(() => {
      const nextValue = getNextExpression();
      hello.value = nextValue;
    }, intervalTime);
  }
});

// -- scrollInto --
const scrollInto = (id) => {
  document.getElementById(id).scrollIntoView({
    behavior: "smooth",
    block: "start",
  });
};

const setIntersectionObserver = () => {
  const sections = document.querySelectorAll(".--content__section");

  const observerOptions = {
    root: document.querySelector(".--container-scroll"),
  };

  const observer = new IntersectionObserver((entries) => {
    entries?.forEach((entry) => {
      const targetLink = document.querySelector(
        `a[data-target="${entry.target.id}"]`
      );

      if (entry.isIntersecting) {
        // const allLinks = document.querySelectorAll("a[data-target]");
        // allLinks.forEach((anchor) => {
        //   anchor.classList.remove("--active");
        // });
        targetLink?.classList.add("--active");
      } else {
        targetLink?.classList.remove("--active");
      }
    });
  }, observerOptions);

  sections.forEach((section) => observer.observe(section));
};

// -- scrollers --

const setScrollerAnimation = () => {
  const scrollers = document.querySelectorAll(".scroller");
  scrollers.forEach((scroller) => {
    const scrollerInner = scroller.querySelector(".scroller__inner");
    const scrollerContent = [...scrollerInner.children];

    scrollerContent.forEach((item) => {
      const duplicatedItem = item.cloneNode(true);
      duplicatedItem.setAttribute("aria-hidden", true);
      scrollerInner.appendChild(duplicatedItem);
    });
  });
};

// -- hola animation --

const setHelloAnimation = () => {
  hasAnimatedHello.value = true;
  const landing = document.querySelector(".landing");
  landing?.classList.add("--zoom-out");
};

const disableHelloAnimation = () => {
  const landing = document.querySelector(".landing");
  landing?.classList.remove("--zoom-out");
  landing?.remove();
};

const setAboutMeAnimation = () => {
  const aboutMe = document.querySelector(".about-me");
  aboutMe.classList.add("--zoom-in");
  setTimeout(() => {
    isAnimating.value = false;
    disableHelloAnimation();
    disableAboutMeAnimation();
    setIntersectionObserver();
  }, 1500);
};

const disableAboutMeAnimation = () => {
  const stack = document.querySelector(".about-me");
  stack.classList.remove("--zoom-in");
  stack.style.setProperty("position", "relative");
  stack.style.setProperty("visibility", "visible");
};

// -- cursor --
const setCandle = () => {
  const candle = document.getElementById("candle");
  document.documentElement.addEventListener(
    "mousemove",
    function handleMouseMove(event) {
      candle.style.setProperty("--light-position-y", event.clientY + "px");
      candle.style.setProperty("--light-position-x", event.clientX + "px");
    }
  );
};

// -- scroll --
const hasAnimatedHello = ref(false);
const isAnimating = ref(false);
const handlerScroll = () => {
  if (!hasAnimatedHello.value) {
    isAnimating.value = true;
    setHelloAnimation();
    setTimeout(() => {
      setAboutMeAnimation();
      window.removeEventListener("wheel", handlerScroll, false);
      window.removeEventListener("touchmove", handlerScroll, false);
    }, 800);
  }
};
const setScrollFunctions = () => {
  window.addEventListener("wheel", handlerScroll, false);
  window.addEventListener("touchmove", handlerScroll, false);
};

const loading = ref(true);
import { onMounted } from "vue";
import TypescriptIcon from "./components/icons/TypescriptIcon.vue";
import ReactIcon from "./components/icons/ReactIcon.vue";
onMounted(() => {
  // setCandle();
  setScrollFunctions();
  setScrollerAnimation();
  loading.value = false;

  setTimeout(() => {
    document.querySelector(".mouse").classList.add("--zoom-in");
  }, 3000);
});
</script>

<template>
  <!-- <div v-if="!isMobile" id="candle" /> -->
  <div class="--stripe-bg" />
  <main class="main">
    <div class="content">
      <section class="landing">
        <div class="hello--wrapper">
          <transition name="slide-fade">
            <p class="--hello" :key="hello">
              {{ hello }}
            </p>
          </transition>
        </div>

        <transition name="slide-fade-inverted">
          <p class="--description" :key="description">
            {{ description }}
          </p>
        </transition>
        <div class="mouse" />
      </section>
      <section class="about-me">
        <div class="wrapper">
          <div class="--name">
            <h1><strong>DANIEL LOPEZ</strong></h1>
            <h2>Front End Engineer</h2>
            <p>Construyo webs accesibles, escalables y responsivas.</p>
            <ul v-if="!isMobile" class="--router">
              <li>
                <a
                  @click="scrollInto('about-me')"
                  data-target="about-me"
                  class="--active"
                  >SOBRE MI</a
                >
              </li>
              <li>
                <a @click="scrollInto('proyects')" data-target="proyects"
                  >PROYECTOS</a
                >
              </li>
              <li>
                <a @click="scrollInto('stack')" data-target="stack">STACK</a>
              </li>
              <li>
                <a @click="scrollInto('contacto')" data-target="contacto"
                  >CONTACTO</a
                >
              </li>
            </ul>
            <section class="--icons">
              <a href="/DANIEL LOPEZ MARTINEZ CV 2025.pdf" download
                ><c-v-icon
              /></a>
              <a href="https://www.linkedin.com/in/dlmjob/" target="_blank"
                ><linkedn-icon
              /></a>
            </section>
          </div>
          <div class="--container-scroll">
            <section id="about-me" class="--content__section">
              <p class="--sticky-header" id="about-me">Sobre mi</p>
              <p>
                Soy desarrollador web especializado en Frontend. Diseño y
                gestiono la arquitectura de aplicaciones web, creando
                componentes personalizados y optimizando el flujo de desarrollo.
                Mi enfoque actual está en construir experiencias accesibles que
                no solo sean visualmente atractivas, sino también
                meticulosamente diseñadas para maximizar el rendimiento y la
                usabilidad.
              </p>
              <p>
                Actualmente trabajo en <a>NEORIS</a> como coordinador/arquitecto
                de Front End. Me encargo del desarrollo de la arquitectura de un
                gestor procesal web para el Ministerio de Justicia, además de
                coordinar al equipo de desarrollo y el reparto de tareas.
              </p>
              <p>
                A lo largo de mi trayectoria, he tenido la oportunidad de
                desarrollar software para diversos sectores, desde el sector
                bancario, sector comercial o el sector público.
              </p>
              <p>
                Me considero una persona a la que le apasiona su trabajo y es
                responsable con él, sigo teniendo el mismo hambre de aprender y
                mejorar que cuando empecé, además de preocuparme por el trabajo
                eficaz y sobretodo, bien hecho.
              </p>
            </section>
            <section id="proyects" class="--content__section">
              <p class="--sticky-header" id="proyects">Proyectos</p>
              <div class="project--wrapper">
                <span class="project--time">2023 - Presente</span>
                <div class="project--info">
                  <span class="project--title"
                    >Exp Frontend Engineer - Ministerio Justicia</span
                  >
                  <p class="project--description">
                    Desarrollo y coordinación de un gestor procesal para el
                    Ministerio de Justicia Español, construcción de la
                    arquitectura frontal de éste, los componentes utilizados,
                    tests unitarios y algoritmos en Cypress para la utilización
                    por parte del equipo de QA. Integración dinámica con Backend
                    y gestión de las tareas del grupo de desarrollo.
                  </p>
                  <p class="project--description">
                    Componentes a medida del negocio, formularios parametrizados
                    con validaciones reactivas, pre fetching de datos para
                    asegurar una carga rápida de datos además de una store que
                    nos permitía manejar los datos desde cualquier vista para
                    evitar requests innecesarias.
                  </p>

                  <div class="scroller">
                    <div class="scroller__inner">
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <javascript-icon class="--icon no-glow" />
                        </div>
                        <span>Javascript ES6+</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <html-icon class="--icon no-glow" />
                        </div>
                        <span>HTML</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <css-icon class="--icon no-glow" />
                        </div>
                        <span>CSS</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <vue-icon class="--icon no-glow" />
                        </div>
                        <span>Vue</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <vite-icon class="--icon no-glow" />
                        </div>
                        <span>Vite</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="project--wrapper">
                <span class="project--time">2022 - 2023</span>
                <div class="project--info">
                  <span class="project--title">Frontend Engineer - Banca</span>
                  <p class="project--description">
                    Desarrollo landing pages además de el área de clientes.
                    Construcción de componentes custom, tests unitarios y diseño
                    totalmente responsive para la app móvil que embebía la web
                    además de un doble router en función del ancho de pantalla,
                    state centralizado, securización de rutas y request,
                    interceptores para refresco de token etc...
                  </p>
                  <div class="scroller">
                    <div class="scroller__inner">
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <javascript-icon class="--icon no-glow" />
                        </div>
                        <span>Javascript ES6+</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <html-icon class="--icon no-glow" />
                        </div>
                        <span>HTML</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <css-icon class="--icon no-glow" />
                        </div>
                        <span>CSS</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <vue-icon class="--icon no-glow" />
                        </div>
                        <span>Vue</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <webpack-icon class="--icon no-glow" />
                        </div>
                        <span>Webpack</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="project--wrapper">
                <span class="project--time">2021 - 2022</span>
                <div class="project--info">
                  <span class="project--title">Frontend Engineer - Banca</span>
                  <p class="project--description">
                    Participé en el departamento de arquitectura, orquestando y
                    maquetando componentes para el grupo de desarrollo, así como
                    trabajando activamente en otros proyectos internos como el
                    desarrollo de webs de seguros, landings o otros aplicativos
                    que incluían Microfronts y paquetes de librerias propios de
                    la casa.
                  </p>
                  <div class="scroller">
                    <div class="scroller__inner">
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <typescript-icon class="--icon no-glow" />
                        </div>
                        <span>TypeScript</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <html-icon class="--icon no-glow" />
                        </div>
                        <span>HTML</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <css-icon class="--icon no-glow" />
                        </div>
                        <span>CSS</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <angular-icon class="--icon no-glow" />
                        </div>
                        <span>Angular</span>
                      </div>
                      <div class="scroller__item">
                        <div class="icon--wrapper-s">
                          <webpack-icon class="--icon no-glow" />
                        </div>
                        <span>Webpack</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </section>
            <section id="stack" class="--content__section">
              <p class="--sticky-header" id="stack">Stack</p>
              <div class="stack--wrapper">
                <div style="display: flex; gap: 1rem; justify-content: center">
                  <javascript-icon class="--icon" />
                  <html-icon class="--icon" />
                  <css-icon class="--icon" />
                  <typescript-icon class="--icon" />
                </div>
                <div>
                  <p>
                    Javascript como mi lenguaje principal, no solo lo uso en
                    Frontend, si no también para la creación de servicios con
                    ExpressJS o otro framework de Node o otro tipo de desarrollo
                    de software. HTML5 y CSS es un obligatorio en el mundo web,
                    así que comencé a aprenderlos mediante libros, cursos y la
                    propia experiencia en mi trabajo. Actualmente trabajo con
                    éstos 3 de manera diaria, manteniéndome actualizado en las
                    nuevas features que salen a la luz.
                  </p>
                  <p>
                    También he trabajado con <strong>Typrescript</strong>,
                    desarrollando un
                    <a
                      href="https://www.npmjs.com/package/generator-express-crud"
                      target="_blank"
                      >generador de APIs</a
                    >
                    a través de un esquema de entidades, desarrollado con
                    NodeJS, Express, Docker, MongoDB y Yeoman.
                  </p>
                  <p>
                    No solo en la parte de cliente, también he desarrollado webs
                    en TypeScript con Vue o React.
                  </p>
                </div>
              </div>
              <div class="stack--wrapper">
                <div style="display: flex; gap: 1rem; justify-content: center">
                  <vue-icon class="--icon" />
                  <vite-icon class="--icon" />
                  <pinia-icon class="--icon" />
                </div>
                <div>
                  <p>
                    El framework con el que más experiencia tengo es
                    <strong style="color: #9dd3b6">Vue</strong>, tanto antiguas
                    versiones como las más actuales. Ésto incluye todo el
                    ecosistema que le rodea como, Vuex, Pinia, Vite, Vitest etc.
                    He realizado multitud de proyectos con éste framework, tanto
                    PWAs como SPAs, desde webs complejas hasta lading pages. Es
                    mi ecosistema de preferencia a la hora de arrancar un
                    proyecto, por la versatilidad, robustez y dinamismo que
                    ofrece en el desarrollo de una web. Actualmente es el
                    framework con el que más cómodo me siento.
                  </p>
                </div>
              </div>
              <div class="stack--wrapper">
                <div style="display: flex; gap: 1rem; justify-content: center">
                  <ReactIcon class="--icon" />
                </div>
                <div>
                  <p>
                    Otro framework con el que he trabajado, es
                    <strong style="color: #53c1de">React</strong>, sobre todo en
                    su versión 16 hacia delante. He desarrollado landings o
                    proyectos personales, aunque puedo extrapolar toda mi
                    experiencia con Vue a React, ya que desde un principio me
                    centré en aprender la web manteniendome agnóstico al
                    framework. Desde el uso de todos los hooks, custom hooks,
                    librerias comunes de React como Redux, el cuidado de los
                    rerenders, ciclos de vida, el uso de React.Memo y una buena
                    orquestación de componentes para no comprometer el
                    rendimiento, conozco el framework como para aventurarme a
                    desarrollar cualquier tipo de web con él.
                  </p>
                </div>
              </div>
              <div class="stack--wrapper">
                <div style="display: flex; gap: 1rem; justify-content: center">
                  <angular-icon class="--icon" />
                </div>
                <div>
                  <p>
                    He trabajado también con el framework de
                    <strong style="color: #cc2e34; padding: 0">Angular</strong>
                    realizando webs en una arquitectura Microfront, librerías de
                    componentes para ésta arquitectura etc. También he utilizado
                    librerías del ecosistema como son RxJS o NGRX.
                  </p>
                </div>
              </div>
              <div class="stack--wrapper">
                <div style="display: flex; gap: 1rem">
                  <span>Otras tecnologías</span>
                </div>
                <div class="stack--others">
                  <span class="stack--others__item">Java</span>
                  <span class="stack--others__item">SpringBoot</span>
                  <span class="stack--others__item">Git</span>
                  <span class="stack--others__item">Cypress</span>
                  <span class="stack--others__item">Playwright</span>
                  <span class="stack--others__item">Jest</span>
                  <span class="stack--others__item">Astro</span>
                  <span class="stack--others__item">Docker</span>
                  <span class="stack--others__item">SQL</span>
                  <span class="stack--others__item">MongoDB</span>
                </div>
              </div>
            </section>
            <section id="contacto" class="--content__section">
              <p class="--sticky-header" id="contacto">Contacto</p>
              <form
                class="contact__form"
                action="https://formsubmit.co/daniel.lmjob@gmail.com"
                method="POST"
              >
                <div style="display: flex; gap: 10px; flex-wrap: wrap">
                  <input
                    placeholder="Nombre"
                    class="contact__input"
                    type="text"
                    name="name"
                    required
                  />
                  <input
                    placeholder="Email"
                    class="contact__input"
                    type="email"
                    name="email"
                    required
                  />
                </div>

                <textarea
                  class="contact__textarea"
                  placeholder="Mensaje"
                  name="message"
                  rows="10"
                  required
                ></textarea>
                <button class="contact__button" type="submit">Send</button>
              </form>
            </section>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>

<style lang="scss">
#candle {
  position: fixed;
  width: 100%;
  height: 400%;
  background: radial-gradient(
    300px at var(--light-position-x, 0px) var(--light-position-y, 0px),
    rgba(255, 125, 0, 0.15),
    transparent 80%
  );
  mix-blend-mode: difference;
}

main {
  width: 100%;
  height: 100%;
}

.content {
  position: relative;
  width: 100%;
  height: 100%;
}

.landing {
  left: 0;
  position: fixed;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 20px;

  p {
    color: var(--color-bright);
    font-size: 1rem;
    letter-spacing: 2px;
    mix-blend-mode: difference;
  }

  .hello--wrapper {
    position: relative;
    display: flex;
    justify-content: center;
  }

  .--hello {
    font-size: 3rem;
    letter-spacing: 2px;
    text-wrap: nowrap;
  }

  .--description {
    padding: 0px 2rem;
    text-align: center;
  }

  .mouse {
    visibility: hidden;
    position: absolute;
    bottom: 10%;
    right: 10%;
    height: 45px;
    width: 35px;
    border: 3px solid var(--color-bright);
    border-radius: 100%;
  }

  .mouse::after {
    content: "";
    position: absolute;
    left: 50%;
    top: 50%;
    width: 2px;
    height: 2px;
    background-color: var(--color-bright);
    transform: translateX(-5px);
    animation: up-down 1s infinite linear;
    animation-direction: alternate;
    @keyframes up-down {
      0% {
        transform: translateX(-1px) translateY(-8px);
      }

      50% {
        transform: translateX(-1px) translateY(0);
      }

      100% {
        transform: translateX(-1px) translateY(8px);
      }
    }
  }
}

.about-me {
  visibility: hidden;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  margin: auto;
  color: var(--color-white);

  h1 {
    position: relative;
    font-size: 3rem;
    letter-spacing: 2px;
    padding: 10px 0px;
  }

  a {
    cursor: pointer;
    color: white;
    font-weight: 600px;
  }

  p {
    text-wrap: pretty;
    color: var(--color-bright);
    margin: 20px 0px;
  }

  .wrapper {
    margin: auto;
    width: 100%;
    max-width: 1420px;
    height: 100%;
    gap: 100px;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  }

  .--name {
    padding-left: 1rem;
    padding-right: 1rem;
    display: flex;
    flex-direction: column;
    height: 100%;
    padding-left: 1.5rem;
    padding-right: 1.5rem;
    padding-top: 6rem;
    padding-bottom: 6rem;

    .--icons {
      margin-top: auto;
      display: flex;
      align-items: center;
      gap: 20px;
      svg {
        fill: var(--color-bright);
      }
    }
  }

  .--container-scroll {
    display: flex;
    flex-direction: column;
    gap: 50px;
    padding-left: 1.5rem;
    padding-right: 1.5rem;
    padding-top: 6rem;
    padding-bottom: 6rem;
    max-width: 100%;
    width: 100%;
    overflow-y: auto;
    overflow-x: hidden;
    scroll-behavior: smooth;
  }

  .--content__section {
    scroll-snap-align: start;
    max-width: 100%;
  }

  .--sticky-header {
    font-size: 1.5rem;
  }

  .--container-scroll::-webkit-scrollbar {
    display: none;
  }

  .--router {
    padding-left: 1rem;
    padding-right: 1rem;

    ul {
    }

    li {
      margin: 1rem 0;
      list-style-type: none;

      a {
        max-width: fit-content;
        cursor: pointer;
        display: flex;
        align-items: center;
        gap: 10px;
        color: var(--color-bright);
        opacity: 0.8;

        &::before {
          transition: all 0.2s ease;
          content: "";
          width: 20px;
          height: 1px;
          background-color: var(--color-highlight);
        }

        &:hover {
          opacity: 1;

          &::before {
            transition: all 0.2s ease;
            content: "";
            width: 30px;
            height: 1px;
            background-color: var(--color-highlight);
          }
        }

        &.--active {
          opacity: 1;
          color: white;

          &::before {
            content: "";
            width: 60px;
            height: 1px;
            background-color: var(--color-highlight);
          }
        }
      }
    }
  }
}

.project--wrapper {
  width: 100%;
  margin-bottom: 60px;
  display: grid;
  grid-template-columns: 170px 1fr;
  border-radius: 20px;
  padding: 15px;
  padding-bottom: 70px;

  transition: background-color 0.2s linear;

  &:hover {
    background-color: rgba(50, 50, 50, 0.4);
  }

  .project--info {
    display: flex;
    flex-direction: column;
    width: 100%;
    max-width: 100%;
    position: relative;
  }

  .scroller {
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
    -webkit-perspective: 1000;
    perspective: 1000;
    position: absolute;
    bottom: -65px;
    left: 0;
    overflow: hidden;
    max-width: inherit;
    -webkit-mask: linear-gradient(
      90deg,
      transparent,
      white 20%,
      white 80%,
      transparent
    );
    mask: linear-gradient(
      90deg,
      transparent,
      white 20%,
      white 80%,
      transparent
    );

    .scroller__inner {
      -webkit-transform-style: preserve-3d;
      transform-style: preserve-3d;
      -webkit-backface-visibility: hidden;
      backface-visibility: hidden;
      -webkit-perspective: 1000;
      perspective: 1000;
      width: fit-content;
      animation: scroll 15s linear infinite;
      padding-block: 10px;
      display: flex;
      gap: 1rem;
    }

    .scroller__item {
      -webkit-transform-style: preserve-3d;
      transform-style: preserve-3d;
      -webkit-backface-visibility: hidden;
      backface-visibility: hidden;
      -webkit-perspective: 1000;
      perspective: 1000;
      padding: 10px;
      background-color: rgba(48, 48, 48, 1);
      border-radius: 5px;
      border-width: 1px;
      border-style: solid;
      border-color: var(--color-highlight);
      box-shadow: 0px 0px 10px var(--color-highlight);
      white-space: nowrap;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 1rem;
    }
  }
}

@keyframes scroll {
  to {
    -webkit-transform: translate3d(calc(-50% - 0.5rem), 0, 0);
    transform: translate3d(calc(-50% - 0.5rem), 0, 0);
  }
}

@media only screen and (max-width: 1270px) {
  .project--wrapper {
    grid-template-columns: 1fr;
  }
}

@media only screen and (max-width: 899px) {
  .about-me {
    height: auto;
  }

  .about-me .--name {
    padding-top: 1rem;
    padding-bottom: 0;
  }

  .about-me .--container-scroll {
    max-height: 100%;
    padding-top: 0;
    padding-left: 0;
    padding-right: 0;
    overflow: inherit;

    p {
      padding-left: 1.5rem;
      padding-right: 1.5rem;
    }
  }

  .about-me .wrapper {
    display: block;
    gap: 0px;
    padding-top: 1rem;
    padding-bottom: 1rem;
  }

  .about-me .--sticky-header {
    padding: 10px 0px;
    backdrop-filter: blur(8px);
    color: white;
    position: sticky;
    top: 0;
    z-index: 99;
  }

  .--content__section {
    p {
      padding-left: 1.5rem;
      padding-right: 1.5rem;
    }
    span {
      padding-left: 1.5rem;
      padding-right: 1.5rem;
    }

    .project--wrapper {
      padding-left: 0;
      padding-right: 0;
    }

    .scroller {
      padding-left: 1.5rem;
      padding-right: 1.5rem;
    }
  }

  .contact__form {
    padding-left: 1.5rem;
    padding-right: 1.5rem;
  }

  .stack--others {
    gap: 0;
  }

  .stack--others {
    padding-left: 1rem;
    padding-right: 1rem;
  }

  .scroller__item span {
    padding-left: 0;
    padding-right: 1rem;
  }
}

.stack--wrapper {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 1rem 0px;
  padding-bottom: 2rem;
}

.stack--others {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.stack--others__item {
  padding: 10px;
  background-color: rgba(48, 48, 48, 1);
  border-radius: 5px;
  border-width: 1px;
  border-style: solid;
  border-color: var(--color-secondary);
  box-shadow: 0px 0px 10px var(--color-secondary);
  white-space: nowrap;
  gap: 1rem;
}

.--icon {
  width: 100%;
  height: 100%;
  max-width: 100px;
}

.no-glow {
  filter: none !important;
}

.icon--wrapper-s {
  width: 24px;
  height: 24px;
}

.--stack {
  max-height: 200px;
}

.contact__form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.contact__input {
  font-family: "Inter";
  font-size: 1rem;
  color: white;
  border: none;
  padding: 1rem;
  flex: 1;
  height: 40px;
  background-color: rgba(40, 40, 40, 0.5);
}

.contact__textarea {
  font-family: "Inter";
  font-size: 1rem;
  color: white;
  border: none;
  padding: 1rem;
  flex: 1;
  background-color: rgba(40, 40, 40, 0.5);
  resize: none;
}

.contact__button {
  cursor: pointer;
  background-color: var(--color-highlight);
  font-family: "Inter";
  font-size: 1rem;
  padding: 1rem;
  flex: 1;
}
</style>
