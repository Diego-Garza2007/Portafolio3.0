<script setup>
import { ref } from "vue";

const showModal = ref(false);
const modalTitle = ref("");
const modalContent = ref("");
const selectedProject = ref(null);

const projects = ref([
  {
    id: 1,
    title: "Emined Scents",
    description:
      "Una Landing page llamativa para atraer clientes usando animaciones gsap",
    video: "/ProjectsVideos/EminedScents.mp4",
    technologies: [
      { name: "Vue.js", icon: "logos:vue" },
      { name: "GSAP", icon: "logos:greensock" },
    ],
    liveLink: "https://eminedscents.info",
    githubLink: null,
    tag: "Landing Page",
  },
  {
    id: 2,
    title: "Vue App Notes",
    description:
      "Aplicación de gestión de tareas con funcionalidades de colaboración en tiempo real y seguimiento de proyectos.",
    video: "/ProjectsVideos/VueNotes.mp4",
    technologies: [
      { name: "Vue.js", icon: "logos:vue" },
      { name: "Vuetify", icon: "logos:vuetifyjs" },
      { name: "Firebase", icon: "logos:firebase" },
    ],
    liveLink: "https://vuenotes-deploy.netlify.app/",
    githubLink: "https://github.com/Diego-Garza2007/Vue-Notes",
    tag: "Full Stack",
  },
  {
    id: 3,
    title: "FinMty",
    description:
      "App para manejar finanzas y ver estadísticas enfocadas en el estado de Nuevo León.",
    video: "/ProjectsVideos/FinMty.mp4",
    technologies: [
      { name: "Vue.js", icon: "logos:vue" },
      { name: "Vuetify", icon: "logos:vuetifyjs" },
      { name: "Node", icon: "simple-icons:nodedotjs", white: true },
      { name: "Express", icon: "simple-icons:express", white: true },
      { name: "Mongo", icon: "simple-icons:mongodb" },
      { name: "Chart.js", icon: "logos:chartjs" },
      { name: "Sass", icon: "logos:sass" },
    ],
    liveLink: "https://fin-mty.vercel.app/",
    githubLink: null,
    tag: "Finance",
  },
  {
    id: 4,
    title: "API de Bebidas",
    description:
      "Mi primer proyecto usando una API con Vue.js y Tailwind, manejando estados con Pinia para la sección de favoritos.",
    video: "/ProjectsVideos/ApiBebidas.mp4",
    technologies: [
      { name: "HTML", icon: "logos:html-5" },
      { name: "CSS3", icon: "logos:css-3" },
      { name: "Javascript", icon: "logos:javascript" },
      { name: "Netlify", icon: "simple-icons:netlify", white: true },
    ],
    liveLink: "https://bebidas-api-vue.netlify.app/",
    githubLink: "https://github.com/Diego-Garza2007/API-bebidas-vue",
    tag: "Frontend",
  },
]);

const centralProject = ref({
  id: "central",
  title: "Plataforma Secundaria",
  description:
    "Plataforma para una escuela secundaria con panel de administración enlazado con sistema RFID para contar asistencias y mostrar calificaciones a alumnos, con función de blogs a modo de comunicados y páginas de presentación.",
  video: "/ProjectsVideos/Sec25.mp4",
  technologies: [
    { name: "Vue 3", icon: "logos:vue" },
    { name: "Node", icon: "simple-icons:nodedotjs", white: true },
    { name: "Express", icon: "simple-icons:express", white: true },
    { name: "PostgreSQL", icon: "logos:postgresql" },
    { name: "GSAP", icon: "logos:greensock" },
  ],
  liveLink: "https://secundariatecnica25.org",
  githubLink: null,
  tag: "Featured Project",
});

const hoveredCard = ref(null);

function openModal(id) {
  let project;
  if (id === "central") {
    project = centralProject.value;
  } else {
    project = projects.value.find((p) => p.id === id);
  }
  selectedProject.value = project;
  modalTitle.value = project.title;
  modalContent.value = project.description;
  showModal.value = true;
}

function closeModal() {
  showModal.value = false;
  selectedProject.value = null;
}

function openLink(url) {
  if (url) window.open(url, "_blank");
}


</script>

<template>
  <main class="projects-page">

    <!-- Back Arrow -->
    <div class="back-arrow">
      <NuxtLink to="/" class="back-link">
        <Icon name="mdi:arrow-left" size="20" />
        <span>Volver</span>
      </NuxtLink>
    </div>

    <!-- Header -->
    <header class="page-header">
      <p class="page-eyebrow">Portafolio</p>
      <h1 class="page-title">Proyectos<span class="dot">.</span></h1>
      <p class="page-subtitle">Trabajo seleccionado — diseño & desarrollo</p>
    </header>

    <!-- Grid de proyectos -->
    <div class="projects-wrapper">
      <div class="projects-grid">
        <div
          class="project-card"
          v-for="(project, index) in projects"
          :key="'card-' + project.id"
          :style="{ '--delay': index * 0.08 + 's' }"
          @mouseenter="hoveredCard = project.id"
          @mouseleave="hoveredCard = null"
        >
          <div class="card-inner">
            <div class="card-media">
              <video autoplay muted loop playsinline disablePictureInPicture class="card-video">
                <source :src="project.video" type="video/mp4" />
              </video>
              <div class="card-scrim"></div>
            </div>

            <div class="card-info">
              <span class="card-tag">{{ project.tag }}</span>
              <h2 class="card-title">{{ project.title }}</h2>
              <p class="card-desc">{{ project.description }}</p>
            </div>

            <button class="card-cta" @click="openModal(project.id)">
              <span>Ver proyecto</span>
              <Icon name="mdi:arrow-top-right" size="16" />
            </button>
          </div>
        </div>
      </div>

      <!-- Featured project -->
      <div
        class="featured-card"
        @mouseenter="hoveredCard = 'central'"
        @mouseleave="hoveredCard = null"
      >
        <div class="featured-inner">
          <div class="featured-media">
            <video autoplay muted loop playsinline disablePictureInPicture class="card-video">
              <source :src="centralProject.video" type="video/mp4" />
            </video>
            <div class="card-scrim featured-scrim"></div>
          </div>

          <div class="featured-content">
            <div class="featured-left">
              <span class="card-tag tag-featured">{{ centralProject.tag }}</span>
              <h2 class="featured-title">{{ centralProject.title }}</h2>
            </div>
            <div class="featured-right">
              <p class="featured-desc">{{ centralProject.description }}</p>
              <div class="featured-tech">
                <span v-for="tech in centralProject.technologies" :key="tech.name" class="tech-pill">
                  <Icon :name="tech.icon" size="14" :style="tech.white ? { color: '#fff' } : {}" />
                  {{ tech.name }}
                </span>
              </div>
              <button class="card-cta featured-cta" @click="openModal('central')">
                <span>Ver proyecto</span>
                <Icon name="mdi:arrow-top-right" size="16" />
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal -->
    <Transition name="modal">
      <div v-if="showModal && selectedProject" class="modal-overlay" @click.self="closeModal">
        <div class="modal-box">
          <button class="modal-close" @click="closeModal">
            <Icon name="mdi:close" size="18" />
          </button>

          <div class="modal-layout">
            <!-- Video side -->
            <div class="modal-media">
              <video autoplay muted loop playsinline disablePictureInPicture class="modal-video">
                <source :src="selectedProject.video" type="video/mp4" />
              </video>
            </div>

            <!-- Info side -->
            <div class="modal-info">
              <div class="modal-top">
                <span class="modal-tag">{{ selectedProject.tag }}</span>
                <h2 class="modal-title">{{ modalTitle }}</h2>
                <p class="modal-description">{{ modalContent }}</p>
              </div>

              <div class="modal-mid">
                <p class="section-label">Stack tecnológico</p>
                <div class="tech-grid">
                  <div
                    v-for="tech in selectedProject.technologies"
                    :key="tech.name"
                    class="tech-chip"
                  >
                    <Icon :name="tech.icon" size="22" :style="tech.white ? { color: '#ffffff' } : {}" />
                    <span>{{ tech.name }}</span>
                  </div>
                </div>
              </div>

              <div class="modal-actions" :class="{ 'single': !selectedProject.githubLink }">
                <button @click="openLink(selectedProject.liveLink)" class="action-btn primary">
                  <Icon name="mdi:web" size="18" />
                  Ver sitio web
                </button>
                <button
                  v-if="selectedProject.githubLink"
                  @click="openLink(selectedProject.githubLink)"
                  class="action-btn secondary"
                >
                  <Icon name="mdi:github" size="18" />
                  Código fuente
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </main>
</template>


<style scoped lang="scss">
@use '../assets/styles/projects.scss';
</style>