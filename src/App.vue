<script setup>
import { ref } from "vue";
import LinkednIcon from "./components/icons/LinkednIcon.vue";
import CVIcon from "./components/icons/CVIcon.vue";
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
    block: "center",
  });
};

const setIntersectionObserver = () => {
  const sections = document.querySelectorAll(".--content__section");

  const observerOptions = {
    root: document.querySelector(".--container-scroll"),
    threshold: 0.8,
  };

  const observer = new IntersectionObserver((entries) => {
    entries?.forEach((entry) => {
      const targetLink = document.querySelector(
        `a[data-target="${entry.target.id}"]`
      );

      if (entry.isIntersecting) {
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
  landing.classList.add("--zoom-out");
};

const disableHelloAnimation = () => {
  const landing = document.querySelector(".landing");
  landing.classList.remove("--zoom-out");
  landing.remove();
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
      window.addEventListener("touchmove", handlerScroll, 1000);
    }, 800);
  }
};
const setScrollFunctions = () => {
  if (!isMobile.value) {
    window.addEventListener("wheel", handlerScroll, false);
  } else {
    window.addEventListener("touchmove", handlerScroll, 1000);
  }
};

const loading = ref(true);
import { onMounted } from "vue";
onMounted(() => {
  setCandle();
  setScrollFunctions();
  setScrollerAnimation();
  loading.value = false;

  setTimeout(() => {
    document.querySelector(".mouse").classList.add("--zoom-in");
  }, 3000);
});
</script>

<template>
  <div id="candle" />
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
                <a @click="scrollInto('about-me')" data-target="about-me"
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
              <a href="/CV - Daniel Lopez Martinez.pdf" download
                ><c-v-icon
              /></a>
              <a href="https://www.linkedin.com/in/dlmjob/" target="_blank"
                ><linkedn-icon
              /></a>
            </section>
          </div>
          <div class="--container-scroll">
            <section id="about-me" class="--content__section">
              <p class="--sticky-header">Sobre mi</p>
              <p>
                Soy un desarrollador web centrado en el Frontend, construyo y
                manejo la arquitectura de la web, creando componentes customs y
                facilitando el desarrollo en ésta. Recientemente he estado
                trabajando en crear experiencias accesibles que no solo se vean
                bien si no que están meticulosamente hechas para el rendimiento
                y usabilidad.
              </p>
              <p>
                Actualmente trabajo en <a>NEORIS</a> como coordinador/arquitecto
                de Front End. Me encargo del desarrollo de la arquitectura de un
                gestor procesal web para el Ministerio de Justicia, los
                componentes de éste y de la creación de herramientas que mejoren
                la experiencia de desarrollo. Además, coordino el desarrollo de
                un equipo de 4 personas front y me aseguro de que la
                comunicación con la parte back sea eficiente y dinámica.
              </p>
              <p>
                En el pasado he tenido la oportunidad de crear software para
                distintos sectores -- desde el sector bancario hasta el sector
                público. He trabajado con webs PWA, SPA e incluso con
                MicroFronts usando <a>Webpack 5 MF</a>.
              </p>
              <p>
                En mi tiempo libre, me podrás encontrar en la pista de pádel,
                pasando tiempo de calidad con mi pareja, jugando a algún
                videojuego o bebiendo cerveza con amigos.
              </p>
            </section>
            <section id="proyects" class="--content__section">
              <p class="--sticky-header">Proyects</p>
              <div class="project--wrapper">
                <span class="project--time">2023 - Present</span>
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
                  <div class="scroller">
                    <div class="scroller__inner">
                      <span class="scroller__item">Javascript ES6+</span>
                      <span class="scroller__item">HTML5</span>
                      <span class="scroller__item">CSS3</span>
                      <span class="scroller__item">VUE3</span>
                      <span class="scroller__item">Vite</span>
                      <span class="scroller__item">Cypress</span>
                      <span class="scroller__item">Git</span>
                    </div>
                  </div>
                </div>
              </div>
              <div class="project--wrapper">
                <span class="project--time">2022 - 2023</span>
                <div class="project--info">
                  <span class="project--title">Frontend Engineer - Banca</span>
                  <p class="project--description">
                    Desarrollo de la landing page además de el área de clientes.
                    Construcción de componentes custom, tests unitarios y diseño
                    totalmente responsive para la app móvil.
                  </p>
                  <div class="scroller">
                    <div class="scroller__inner">
                      <span class="scroller__item">Javascript ES6+</span>
                      <span class="scroller__item">HTML5</span>
                      <span class="scroller__item">CSS3</span>
                      <span class="scroller__item">VUE2/3</span>
                      <span class="scroller__item">Webpack</span>
                      <span class="scroller__item">Jest</span>
                      <span class="scroller__item">Git</span>
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
                      <span class="scroller__item">Javascript ES6+</span>
                      <span class="scroller__item">HTML5</span>
                      <span class="scroller__item">CSS3</span>
                      <span class="scroller__item">Angular</span>
                      <span class="scroller__item">MicroFronts</span>
                      <span class="scroller__item">Webpack 5</span>
                      <span class="scroller__item">WebComponents</span>
                      <span class="scroller__item">Babel</span>
                      <span class="scroller__item">Git</span>
                    </div>
                  </div>
                </div>
              </div>
              <div class="project--wrapper">
                <span class="project--time">2020 - 2021</span>
                <div class="project--info">
                  <span class="project--title"
                    >Jr Fullstack Engineer - SMS</span
                  >
                  <p class="project--description">
                    Desarrollo de aplicativos para el servicio de salud
                    Murciano, desde backend con springboot a frontend con JSF.
                  </p>
                  <div class="scroller">
                    <div class="scroller__inner">
                      <span class="scroller__item">Java</span>
                      <span class="scroller__item">SpringBoot</span>
                      <span class="scroller__item">JSF</span>
                      <span class="scroller__item">SVN</span>
                      <span class="scroller__item">Apache</span>
                    </div>
                  </div>
                </div>
              </div>
            </section>
            <section id="stack" class="--content__section">
              <p class="--sticky-header">Stack</p>
              <p>
                Empecé en el desarrollo como backend con Java, por tanto
                entiendo tanto de como se conforman las APIs como del propio
                lenguaje, más tarde me atreví con el Frontend y comencé mi viaje
                con Javascript, HTML y CSS. Aunque son 3 elementos muy amplios,
                intento mantenerme al día con las mejoras y nuevas
                implementaciones de cada uno de éstos, tal y como se puede ver
                por ejemplo en éste portfolio con algunas nuevas
                implementaciones de CSS3. Considero a Javascript mi lenguaje
                principal de programación, ya que con él he programado no solo
                Frontend si no también backend con ExpressJS y NodeJS, incluso
                herramientas personales como
                <a
                  href="https://www.npmjs.com/package/generator-express-crud"
                  target="_blank"
                  >generadores de APIs</a
                >
                automáticos a través de una entrada de entidades con un .yaml,
                plantillas en Typescript con EJS y prompts con Yeoman.
              </p>
              <p>
                El framework con el que más experiencia tengo es sin duda
                <span style="color: #9dd3b6">Vue</span>, tanto antiguas
                versiones como las más actuales. Ésto incluye todo el ecosistema
                que le rodea como Vuex, Pinia, Vite, Vitest... También me he
                salido un poco del ecosistema usando Vue en versiones más
                antiguas junto con Webpack o Jest. He realizado multitud de
                proyectos con éste framework, explorando cada una de las
                opciones que te da, así como he realizado cursos, leído libros y
                hecho mis propios experimentos para ampliar mi conocimiento con
                Vue.
              </p>
              <p>
                También tengo conocimientos con frameworks como Nuxt para crear
                webs SSR, posicionamiento SEO u herramientas como StoryBook.
              </p>
              <p>
                He trabajado también con el framework de
                <span style="color: #cc2e34">Angular</span> el cual me ha
                llamado mucho en sus últimas versiones dada la experiencia de
                desarrollo más cercana a otros frameworks como Vue. Laboralmente
                he trabajado con versiones más antiguas de Angular y en concreto
                usando Microfronts con Webpack 5. También conozco el ecosistema
                Angular como las librerias RXJS o Ngrx.
              </p>
              <p>
                En general soy una persona bastante curiosa que me gusta, al
                menos, conocer los distintos frameworks o herramientas que me
                ofrece la web. Aunque mi especialización sea con Vue, puedo ser
                muy resolutivo con cualquier framework dado mi rápido
                aprendizaje y el gusto que tengo por conocer nuevas
                herramientas.
              </p>
              <p>
                Dentro de mis proyectos siempre me gusta trabajar con
                componentes customs, patrones de diseño, clean code e intentar
                mejorar la experiencia de desarrollo dentro del proyecto.
                También he implementado sistemas de login cifrados, SSO,
                interceptores de request ( Axios ), rutas securizadas etc...
              </p>
              <p>
                Ésto es solo un resumen de lo que conozco, cualquier pregunta no
                dudes en ponerte en contacto conmigo.
              </p>
            </section>
            <section id="contacto" class="--content__section">
              <p class="--sticky-header">Contacto</p>
              <form
                class="contact__form"
                action="https://formsubmit.co/05508ba02217d8faa8aaffa6b374a473"
                method="POST"
              >
                <div style="display: flex; gap: 10px; flex-wrap: wrap">
                  <input
                    placeholder="Name"
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
  gap: 20px;

  h1 {
    position: relative;
    font-size: 3rem;
    letter-spacing: 4px;
    padding: 20px 0px;
  }

  a {
    cursor: pointer;
    color: white;
    font-weight: 600px;
  }

  p {
    text-wrap: pretty;
    color: var(--color-bright);
    margin: 30px 0px;
  }

  .wrapper {
    margin: auto;
    width: 100%;
    max-width: 1520px;
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
    margin-top: 2rem;
    padding-left: 1rem;
    padding-right: 1rem;

    ul {
    }

    li {
      margin: 1rem 0;
      list-style-type: none;

      a {
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
  grid-template-columns: 150px 1fr;
  border-radius: 20px;
  padding: 15px;
  padding-bottom: 50px;

  transition: background-color 0.2s linear;

  &:hover {
    background-color: rgba(50, 50, 50, 0.4);
  }

  .project--time {
  }

  .project--description {
  }

  .project--info {
    display: flex;
    flex-direction: column;
    width: 100%;
    max-width: 100%;
    position: relative;
  }

  .scroller {
    -webkit-transform-style: preserve-3d;
    -webkit-backface-visibility: hidden;
    transform-style: preserve-3d;
    backface-visibility: hidden;
    position: absolute;
    bottom: -50px;
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
      -webkit-backface-visibility: hidden;
      transform-style: preserve-3d;
      backface-visibility: hidden;
      width: fit-content;
      animation: scroll 15s linear infinite;
      padding-block: 10px;
      display: flex;
      gap: 1rem;
    }

    .scroller__item {
      -webkit-transform-style: preserve-3d;
      -webkit-backface-visibility: hidden;
      transform-style: preserve-3d;
      backface-visibility: hidden;
      padding: 10px;
      background-color: var(--color-secondary);
      box-shadow: 0px 2px 4px 1px rgba(0, 0, 0, 0.2);
      border-radius: 20px;
      white-space: nowrap;
    }

    .scroller__inner:hover {
      animation-play-state: paused;
    }
  }
}

@keyframes scroll {
  to {
    translate: calc(-50% - 0.5rem);
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
    padding-bottom: 1rem;
  }

  .about-me .--container-scroll {
    max-height: 100%;
    padding-top: 1rem;
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
  background-color: var(--color-highlight);
  font-family: "Inter";
  font-size: 1rem;
  padding: 1rem;
  flex: 1;
}

.--scrollable {
  width: 100%;
  height: 2000px;
}
</style>
