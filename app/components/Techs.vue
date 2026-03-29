<template>
  <section class="tech-section">
    <p class="tech-section__eyebrow">Stack</p>
    <h2 class="tech-section__title">Technology I Use</h2>

    <div class="orbit-wrapper">

      <!-- Panel central -->
      <div class="orbit-center">
        <Transition name="fade-up" mode="out-in">
          <div class="orbit-center__inner" :key="selected.name">
            <Icon :name="selected.icon" class="orbit-center__bg-logo" />
            <div class="orbit-center__content">
              <Icon :name="selected.icon" class="orbit-center__icon" />
              <h3 class="orbit-center__name">{{ selected.name }}</h3>
              <p class="orbit-center__desc">{{ selected.description }}</p>
            </div>
          </div>
        </Transition>
      </div>

      <!-- SVG arcos de nivel -->
      <svg class="level-svg" viewBox="0 0 680 680">
        <!-- Fondos -->
        <circle cx="340" cy="340" r="162" fill="none" stroke="rgba(255,255,255,0.05)"
          stroke-width="4" stroke-dasharray="763 255" stroke-dashoffset="127" stroke-linecap="round" />
        <circle cx="340" cy="340" r="174" fill="none" stroke="rgba(255,255,255,0.05)"
          stroke-width="4" stroke-dasharray="820 273" stroke-dashoffset="137" stroke-linecap="round" />
        <circle cx="340" cy="340" r="186" fill="none" stroke="rgba(255,255,255,0.05)"
          stroke-width="4" stroke-dasharray="876 292" stroke-dashoffset="146" stroke-linecap="round" />

        <!-- Arco nivel 1 — verde (Beginner) -->
        <circle cx="340" cy="340" r="162" fill="none"
          :stroke="selected.level >= 1 ? '#5cffa6' : 'transparent'"
          stroke-width="4" stroke-dasharray="763 255" stroke-dashoffset="127" stroke-linecap="round"
          class="level-arc" />
        <!-- Arco nivel 2 — amarillo (Intermediate) -->
        <circle cx="340" cy="340" r="174" fill="none"
          :stroke="selected.level >= 2 ? '#f5c542' : 'transparent'"
          stroke-width="4" stroke-dasharray="820 273" stroke-dashoffset="137" stroke-linecap="round"
          class="level-arc" />
        <!-- Arco nivel 3 — rojo (Advanced) -->
        <circle cx="340" cy="340" r="186" fill="none"
          :stroke="selected.level >= 3 ? '#ff5c5c' : 'transparent'"
          stroke-width="4" stroke-dasharray="876 292" stroke-dashoffset="146" stroke-linecap="round"
          class="level-arc" />

        <!-- Anillo exterior decorativo -->
        <circle cx="340" cy="340" r="240" fill="none"
          stroke="rgba(92,255,166,0.06)" stroke-width="1" stroke-dasharray="5 7" />
      </svg>

      <!-- Badge de nivel -->
      <Transition name="fade-up">
        <div class="level-badge" :class="`level-badge--${selected.level}`" :key="'badge-' + selected.name">
          <span class="level-badge__dot" />
          {{ levelLabel(selected.level) }}
        </div>
      </Transition>

      <!-- Nodos orbitales -->
      <button
        v-for="(tech, i) in techs"
        :key="tech.name"
        class="orbit-node"
        :class="{ 'orbit-node--active': selected.name === tech.name }"
        :style="nodeStyle(i)"
        @click="select(tech)"
        @mouseenter="select(tech)"
      >
        <span class="orbit-node__dot" />
        <div class="orbit-node__card">
          <Icon :name="tech.icon" class="orbit-node__logo" />
          <span class="orbit-node__label">{{ tech.name }}</span>
        </div>
        <svg class="orbit-node__line" :style="lineStyle(i)" aria-hidden="true">
          <line x1="0" y1="50%" x2="100%" y2="50%" stroke-width="1" />
        </svg>
      </button>

    </div>

    <!-- Leyenda de niveles -->
    <div class="level-legend">
      <span class="level-legend__item level-legend__item--1">
        <span class="level-legend__dot" />Beginner
      </span>
      <span class="level-legend__item level-legend__item--2">
        <span class="level-legend__dot" />Intermediate
      </span>
      <span class="level-legend__item level-legend__item--3">
        <span class="level-legend__dot" />Advanced
      </span>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const techs = [
  {
    name: 'Vue.js',
    icon: 'simple-icons:vuedotjs',
    level: 2,
    description: 'I use Vue.js for building reactive and modular frontend interfaces with clean component architecture.'
  },
  {
    name: 'JavaScript',
    icon: 'simple-icons:javascript',
    level: 2,
    description: 'JavaScript is my main language for web development, both frontend and backend logic.'
  },
  {
    name: 'Firebase',
    icon: 'simple-icons:firebase',
    level: 1,
    description: 'I use Firebase for backend services like authentication, real-time database, and cloud hosting.'
  },
  {
    name: 'MongoDB',
    icon: 'simple-icons:mongodb',
    level: 2,
    description: 'MongoDB is my preferred database for storing flexible, scalable, and document-based data.'
  },
  {
    name: 'Nuxt',
    icon: 'simple-icons:nuxtdotjs',
    level: 1,
    description: 'Nuxt.js helps me build server-side rendered Vue apps with powerful routing and meta management.'
  },
  {
    name: 'Node.js',
    icon: 'simple-icons:nodedotjs',
    level: 2,
    description: 'Node.js is my backend runtime for building fast REST APIs and server-side applications.'
  },
  {
    name: 'Next.js',
    icon: 'simple-icons:nextdotjs',
    level: 2,
    description: 'Next.js allows me to build fast, SEO-friendly React applications with server-side rendering.'
  },
  {
    name: 'GSAP',
    icon: 'simple-icons:greensock',
    level: 1,
    description: 'I use GSAP to create smooth, high-performance animations and interactive UI experiences.'
  },
  {
    name: 'Vuetify',
    icon: 'simple-icons:vuetify',
    level: 2,
    description: 'Vuetify provides me with a rich set of Material Design components to build polished Vue apps.'
  },
  {
    name: 'PostgreSQL',
    icon: 'simple-icons:postgresql',
    level: 1,
    description: 'PostgreSQL is my go-to relational database for structured data and complex queries.'
  },
  {
    name: 'Python',
    icon: 'simple-icons:python',
    level: 1,
    description: 'I use Python for scripting, automation, and exploring data science and backend concepts.'
  },
  {
    name: 'Linux',
    icon: 'simple-icons:linux',
    level: 2,
    description: 'Linux is my primary development environment, giving me control over servers and tooling.'
  }
]

const levelLabels = { 1: 'Beginner', 2: 'Intermediate', 3: 'Advanced' }
function levelLabel(l) { return levelLabels[l] ?? '—' }

const selected = ref(techs[0])
function select(tech) { selected.value = tech }

const RADIUS = 290
const CENTER = 340

function nodeStyle(i) {
  const angle = (360 / techs.length) * i - 90
  const rad   = (angle * Math.PI) / 180
  const x     = CENTER + RADIUS * Math.cos(rad) - 56
  const y     = CENTER + RADIUS * Math.sin(rad) - 56
  return { left: `${x}px`, top: `${y}px` }
}

const PANEL_R = 230
function lineStyle(i) {
  const angle  = (360 / techs.length) * i - 90
  const len    = RADIUS - PANEL_R - 10
  const rotate = angle + 180
  return {
    width:           `${len}px`,
    transform:       `rotate(${rotate}deg)`,
    transformOrigin: '0% 50%',
    left:            '56px',
    top:             '56px',
  }
}
</script>

<style scoped>
/* ── Variables ── */
.tech-section {
  --c-green:      #5cffa6;
  --c-yellow:     #f5c542;
  --c-red:        #ff5c5c;
  --bg-card:      rgba(15, 18, 15, 0.72);
  --bg-node:      rgba(14, 17, 14, 0.85);
  --border-green: rgba(92, 255, 166, 0.18);
  --border-sub:   rgba(92, 255, 166, 0.07);
  --text-primary: #f0ede8;
  --text-muted:   #888;
  --text-dim:     #777;
  --font:         'Inter', sans-serif;

  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 56px 0 40px;
  font-family: var(--font);
  width: 100%;
}

/* ── Eyebrow / title ── */
.tech-section__eyebrow {
  font-size: 0.78rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--c-green);
  margin: 0 0 10px;
}

.tech-section__title {
  font-size: clamp(2rem, 4vw, 3rem);
  font-weight: 800;
  letter-spacing: -0.03em;
  color: var(--text-primary);
  margin: 0 0 56px;
  line-height: 1;
}
.tech-section__title::after {
  content: '.';
  color: var(--c-green);
}

/* ── Orbit wrapper ── */
.orbit-wrapper {
  position: relative;
  width: 680px;
  height: 680px;
  flex-shrink: 0;
}

/* ── Centro ── */
.orbit-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 300px;
  height: 300px;
  border-radius: 50%;
  background: var(--bg-card);
  backdrop-filter: blur(28px) saturate(160%);
  -webkit-backdrop-filter: blur(28px) saturate(160%);
  border: 1px solid var(--border-green);
  box-shadow:
    0 0 0 8px rgba(92,255,166,0.03),
    0 0 60px rgba(92,255,166,0.05),
    inset 0 1px 0 rgba(92,255,166,0.08);
  z-index: 10;
  overflow: hidden;
}

.orbit-center__inner {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.orbit-center__bg-logo {
  position: absolute;
  inset: 0;
  margin: auto;
  width: 200px;
  height: 200px;
  opacity: 0.04;
  pointer-events: none;
  filter: grayscale(1);
  color: #fff;
}

.orbit-center__content {
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  padding: 20px;
  text-align: center;
}

.orbit-center__icon {
  width: 54px;
  height: 54px;
  color: var(--c-green);
  filter: drop-shadow(0 0 12px rgba(92,255,166,0.3));
}

.orbit-center__name {
  font-size: 1.05rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  color: var(--text-primary);
  margin: 0;
  text-transform: uppercase;
}

.orbit-center__desc {
  font-size: 0.72rem;
  line-height: 1.65;
  color: var(--text-dim);
  margin: 0;
  font-weight: 300;
}

/* ── SVG arcos ── */
.level-svg {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  z-index: 8;
  pointer-events: none;
}

.level-arc {
  transition: stroke 0.4s ease;
}

/* ── Badge de nivel — color según nivel ── */
.level-badge {
  position: absolute;
  top: calc(50% + 102px);
  left: 50%;
  transform: translateX(-50%);
  z-index: 15;
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 4px 14px;
  border-radius: 100px;
  font-size: 0.62rem;
  font-weight: 600;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  white-space: nowrap;
  background: rgba(15,18,15,0.88);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  box-shadow: 0 4px 16px rgba(0,0,0,0.4);
  transition: color 0.3s, border-color 0.3s;
}

/* Verde para Beginner */
.level-badge--1 {
  color: var(--c-green);
  border: 1px solid rgba(92,255,166,0.35);
}
/* Amarillo para Intermediate */
.level-badge--2 {
  color: var(--c-yellow);
  border: 1px solid rgba(245,197,66,0.35);
}
/* Rojo para Advanced */
.level-badge--3 {
  color: var(--c-red);
  border: 1px solid rgba(255,92,92,0.35);
}

.level-badge__dot {
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: currentColor;
  flex-shrink: 0;
  box-shadow: 0 0 6px currentColor;
}

/* ── Nodos ── */
.orbit-node {
  position: absolute;
  width: 112px;
  height: 112px;
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  z-index: 20;
  display: flex;
  align-items: center;
  justify-content: center;
}

.orbit-node__dot {
  position: absolute;
  inset: 0;
  margin: auto;
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: rgba(92,255,166,0.2);
  transition: background 0.25s, transform 0.25s, box-shadow 0.25s;
  z-index: 2;
}

.orbit-node__card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 5px;
  width: 68px;
  height: 68px;
  border-radius: 14px;
  background: var(--bg-node);
  backdrop-filter: blur(16px) saturate(140%);
  -webkit-backdrop-filter: blur(16px) saturate(140%);
  border: 1px solid var(--border-sub);
  transition: border-color 0.25s, box-shadow 0.25s, transform 0.25s;
  z-index: 3;
  flex-shrink: 0;
}

.orbit-node__logo {
  width: 26px;
  height: 26px;
  color: var(--text-muted);
  flex-shrink: 0;
  transition: transform 0.25s, color 0.25s, filter 0.25s;
}

.orbit-node__label {
  font-size: 0.55rem;
  font-weight: 500;
  letter-spacing: 0.06em;
  color: var(--text-muted);
  white-space: nowrap;
  text-transform: uppercase;
  transition: color 0.25s;
}

.orbit-node__line {
  position: absolute;
  height: 2px;
  overflow: visible;
  pointer-events: none;
  z-index: 1;
}
.orbit-node__line line {
  stroke: rgba(92,255,166,0.07);
  transition: stroke 0.25s;
}

.orbit-node:hover .orbit-node__card,
.orbit-node--active .orbit-node__card {
  border-color: rgba(92,255,166,0.28);
  box-shadow:
    0 0 0 1px rgba(92,255,166,0.08),
    0 8px 28px rgba(92,255,166,0.07);
  transform: scale(1.08);
}
.orbit-node:hover .orbit-node__label,
.orbit-node--active .orbit-node__label { color: var(--c-green); }
.orbit-node:hover .orbit-node__logo,
.orbit-node--active .orbit-node__logo {
  transform: scale(1.12);
  color: var(--c-green);
  filter: drop-shadow(0 0 8px rgba(92,255,166,0.3));
}
.orbit-node:hover .orbit-node__dot,
.orbit-node--active .orbit-node__dot {
  background: var(--c-green);
  transform: scale(1.8);
  box-shadow: 0 0 8px rgba(92,255,166,0.5);
}
.orbit-node:hover .orbit-node__line line,
.orbit-node--active .orbit-node__line line {
  stroke: rgba(92,255,166,0.25);
}

/* ── Leyenda ── */
.level-legend {
  display: flex;
  align-items: center;
  gap: 24px;
  margin-top: 24px;
}

.level-legend__item {
  display: flex;
  align-items: center;
  gap: 7px;
  font-size: 0.72rem;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.level-legend__dot {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  flex-shrink: 0;
}

.level-legend__item--1 { color: #5cffa6; }
.level-legend__item--1 .level-legend__dot {
  background: #5cffa6;
  box-shadow: 0 0 6px rgba(92,255,166,0.6);
}

.level-legend__item--2 { color: #f5c542; }
.level-legend__item--2 .level-legend__dot {
  background: #f5c542;
  box-shadow: 0 0 6px rgba(245,197,66,0.6);
}

.level-legend__item--3 { color: #ff5c5c; }
.level-legend__item--3 .level-legend__dot {
  background: #ff5c5c;
  box-shadow: 0 0 6px rgba(255,92,92,0.6);
}

/* ── Transitions ── */
.fade-up-enter-active,
.fade-up-leave-active { transition: all 0.28s cubic-bezier(0.4,0,0.2,1); }
.fade-up-enter-from   { opacity: 0; transform: translateY(10px) scale(0.97); }
.fade-up-leave-to     { opacity: 0; transform: translateY(-8px) scale(0.97); }

/* ── Responsive ── */
@media (max-width: 740px) {
  .orbit-wrapper {
    transform: scale(0.68);
    transform-origin: top center;
    margin-bottom: -140px;
  }
}
@media (max-width: 480px) {
  .orbit-wrapper {
    transform: scale(0.50);
    margin-bottom: -220px;
  }
}
</style>