<template>
  <section class="setup-section">
    <p class="setup-section__eyebrow">Workspace</p>
    <h2 class="setup-section__title">My Setup</h2>

    <!-- ── Card principal: setup actual ── -->
    <div class="setup-current">
      <div class="setup-current__media">
        <img
          :src="current.img"
          :alt="`Setup ${current.year}`"
          class="setup-current__photo"
        />
        <!-- Scrim inferior -->
        <div class="setup-current__scrim" />

        <!-- Badge "Current" -->
        <span class="setup-current__badge">
          <span class="setup-current__badge-dot" />
          Current
        </span>

        <!-- Año flotante -->
        <span class="setup-current__year">{{ current.year }}</span>
      </div>

      <div class="setup-current__info">
        <h3 class="setup-current__name">{{ current.name }}</h3>
        <p class="setup-current__desc">{{ current.description }}</p>

        <!-- Specs -->
        <ul class="setup-current__specs">
          <li
            v-for="spec in current.specs"
            :key="spec.label"
            class="setup-current__spec"
          >
            <Icon :name="spec.icon" class="setup-current__spec-icon" />
            <span class="setup-current__spec-label">{{ spec.label }}</span>
            <span class="setup-current__spec-value">{{ spec.value }}</span>
          </li>
        </ul>

        <button class="setup-current__history-btn" @click="openHistory">
          <Icon name="mdi:history" class="setup-current__btn-icon" />
          View Setup History
          <span class="setup-current__btn-count">{{ history.length }}</span>
        </button>
      </div>
    </div>

    <!-- ── Modal: historial de setups ── -->
    <Transition name="modal">
      <div v-if="showHistory" class="setup-modal" @click.self="closeHistory">
        <div class="setup-modal__box">

          <div class="setup-modal__header">
            <div>
              <p class="setup-modal__eyebrow">Evolution</p>
              <h3 class="setup-modal__title">Setup History</h3>
            </div>
            <button class="setup-modal__close" @click="closeHistory">
              <Icon name="mdi:close" size="18" />
            </button>
          </div>

          <!-- Timeline de setups -->
          <div class="setup-modal__timeline">
            <div
              v-for="(setup, i) in history"
              :key="setup.year"
              class="setup-timeline__item"
              :style="{ '--delay': `${i * 0.06}s` }"
            >
              <!-- Línea vertical -->
              <div class="setup-timeline__line">
                <span class="setup-timeline__dot" />
              </div>

              <div class="setup-timeline__card" @click="previewSetup(setup)">
                <div class="setup-timeline__media">
                  <img
                    :src="setup.img"
                    :alt="`Setup ${setup.year}`"
                    class="setup-timeline__photo"
                  />
                  <span class="setup-timeline__year-badge">{{ setup.year }}</span>
                </div>
                <div class="setup-timeline__info">
                  <h4 class="setup-timeline__name">{{ setup.name }}</h4>
                  <p class="setup-timeline__desc">{{ setup.description }}</p>
                  <div class="setup-timeline__tags">
                    <span
                      v-for="tag in setup.tags"
                      :key="tag"
                      class="setup-timeline__tag"
                    >{{ tag }}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>

        </div>
      </div>
    </Transition>

    <!-- ── Preview setup seleccionado ── -->
    <Transition name="modal">
      <div v-if="previewed" class="setup-preview" @click.self="previewed = null">
        <div class="setup-preview__box">
          <button class="setup-modal__close setup-preview__close" @click="previewed = null">
            <Icon name="mdi:close" size="18" />
          </button>
          <img
            :src="previewed.img"
            :alt="previewed.name"
            class="setup-preview__photo"
          />
          <div class="setup-preview__info">
            <span class="setup-preview__year">{{ previewed.year }}</span>
            <h3 class="setup-preview__name">{{ previewed.name }}</h3>
            <p class="setup-preview__desc">{{ previewed.description }}</p>
          </div>
        </div>
      </div>
    </Transition>

  </section>
</template>

<script setup>
import { ref } from 'vue'

// ── Setup actual ────────────────────────────────────────────
const current = {
  year: '2025',
  name: 'Actual Setup',
  img: '/ActualSetup.jpeg',           // ← cambia por tu ruta real
  description: 'Enfocado en la productividad tratando de ser lo mas minimalista posible al mismo tiempo siendo economico.',
  specs: [
    { icon: 'mdi:monitor',        label: 'Monitor',   value: 'BENQ 24"' },
    { icon: 'mdi:computer',         label: 'CPU',   value: 'Ryzen 5600g' },
    { icon: 'mdi:keyboard',       label: 'Keyboard',  value: 'YUNZII AL66' },
    { icon: 'mdi:mouse',          label: 'Mouse',     value: 'Logitech G502' },
    { icon: 'mdi:headphones',     label: 'Audio',     value: 'SoundPEATS Space Wireless' },
    { icon: 'mdi:chair-rolling',  label: 'Chair',     value: 'Generic Gaming Chair' },
  ]
}

// ── Historial ───────────────────────────────────────────────
const history = [
  {
    year: '2020',
    name: 'The Upgrade',
    img: '/2020setup.jpeg',
    description: 'Mi salto de 30 fps en minecraft a 120fps de las cosas mas bellas de mi vida',
    tags: ['All in one', 'NVIDIA MX 130', 'Stolen chair']
  },
  {
    year: '2015',
    name: 'The Beginning',
    img: '2016setup.png',
    description: 'Inicie con un intel pentium con 4gb de RAM que buenos tiempos.',
    tags: ['All in one PC', 'Intel Pentium', 'Grandma chair']
  },
]

const showHistory = ref(false)
const previewed   = ref(null)

function openHistory()       { showHistory.value = true }
function closeHistory()      { showHistory.value = false; previewed.value = null }
function previewSetup(setup) { previewed.value = setup }
</script>

<style scoped>
/* ── Variables ── */
.setup-section {
  --accent:        #5cffa6;
  --accent-dim:    rgba(92, 255, 166, 0.10);
  --accent-border: rgba(92, 255, 166, 0.20);
  --bg-card:       rgba(15, 18, 15, 0.75);
  --bg-node:       rgba(12, 15, 12, 0.88);
  --text-primary:  #f0ede8;
  --text-muted:    #888;
  --text-dim:      #666;
  --font:          'Inter', sans-serif;

  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 56px 0 40px;
  font-family: var(--font);
  width: 100%;
}

/* ── Eyebrow / Title ── */
.setup-section__eyebrow {
  font-size: 0.78rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--accent);
  margin: 0 0 10px;
  align-self: center;
}

.setup-section__title {
  font-size: clamp(2rem, 4vw, 3rem);
  font-weight: 800;
  letter-spacing: -0.03em;
  color: var(--text-primary);
  margin: 0 0 40px;
  line-height: 1;
}
.setup-section__title::after {
  content: '.';
  color: var(--accent);
}

/* ── Card actual ── */
.setup-current {
  width: 100%;
  max-width: 760px;
  border-radius: 20px;
  overflow: hidden;
  background: var(--bg-card);
  backdrop-filter: blur(24px) saturate(160%);
  -webkit-backdrop-filter: blur(24px) saturate(160%);
  border: 1px solid var(--accent-border);
  box-shadow:
    0 0 0 1px rgba(0,0,0,0.4),
    0 24px 80px rgba(0,0,0,0.5),
    inset 0 1px 0 rgba(92,255,166,0.07);
  animation: fadeUp 0.6s both 0.1s;
}

/* ── Media ── */
.setup-current__media {
  position: relative;
  height: 340px;
  overflow: hidden;
}

.setup-current__photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.6s cubic-bezier(0.23,1,0.32,1);
}
.setup-current:hover .setup-current__photo {
  transform: scale(1.03);
}

.setup-current__scrim {
  position: absolute;
  inset: 0;
  background: linear-gradient(to bottom, transparent 40%, rgba(10,10,10,0.85) 100%);
}

.setup-current__badge {
  position: absolute;
  top: 16px;
  left: 16px;
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 5px 12px;
  border-radius: 100px;
  font-size: 0.65rem;
  font-weight: 600;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--accent);
  background: rgba(15,18,15,0.85);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(92,255,166,0.35);
}

.setup-current__badge-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--accent);
  box-shadow: 0 0 8px rgba(92,255,166,0.8);
  animation: pulse 2s ease-in-out infinite;
}

.setup-current__year {
  position: absolute;
  bottom: 16px;
  right: 20px;
  font-size: 5rem;
  font-weight: 800;
  letter-spacing: -0.04em;
  color: rgba(240,237,232,0.06);
  line-height: 1;
  pointer-events: none;
  user-select: none;
}

/* ── Info ── */
.setup-current__info {
  padding: 28px 28px 24px;
}

.setup-current__name {
  font-size: 1.4rem;
  font-weight: 700;
  color: var(--text-primary);
  margin: 0 0 10px;
  letter-spacing: -0.02em;
}

.setup-current__desc {
  font-size: 0.88rem;
  color: var(--text-muted);
  line-height: 1.65;
  font-weight: 300;
  margin: 0 0 24px;
}

/* ── Specs ── */
.setup-current__specs {
  list-style: none;
  padding: 0;
  margin: 0 0 24px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 10px;
}

.setup-current__spec {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 14px;
  border-radius: 10px;
  background: rgba(92,255,166,0.03);
  border: 1px solid rgba(92,255,166,0.08);
  transition: border-color 0.2s, background 0.2s;
}
.setup-current__spec:hover {
  border-color: rgba(92,255,166,0.2);
  background: rgba(92,255,166,0.06);
}

.setup-current__spec-icon {
  width: 16px;
  height: 16px;
  color: var(--accent);
  flex-shrink: 0;
  opacity: 0.8;
}

.setup-current__spec-label {
  font-size: 0.7rem;
  color: var(--text-dim);
  letter-spacing: 0.06em;
  text-transform: uppercase;
  flex-shrink: 0;
}

.setup-current__spec-value {
  font-size: 0.78rem;
  color: var(--text-primary);
  font-weight: 500;
  margin-left: auto;
  text-align: right;
}

/* ── Botón historial ── */
.setup-current__history-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 11px 20px;
  border-radius: 10px;
  background: rgba(92,255,166,0.06);
  border: 1px solid rgba(92,255,166,0.2);
  color: var(--accent);
  font-family: var(--font);
  font-size: 0.85rem;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.25s, border-color 0.25s, transform 0.2s;
  letter-spacing: 0.02em;
}
.setup-current__history-btn:hover {
  background: rgba(92,255,166,0.12);
  border-color: rgba(92,255,166,0.4);
  transform: translateY(-2px);
}

.setup-current__btn-icon {
  width: 16px;
  height: 16px;
}

.setup-current__btn-count {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: rgba(92,255,166,0.15);
  font-size: 0.7rem;
  font-weight: 700;
  line-height: 1;
}

/* ── Modal overlay ── */
.setup-modal {
  position: fixed;
  inset: 0;
  background: rgba(4,6,4,0.78);
  backdrop-filter: blur(20px) saturate(120%);
  -webkit-backdrop-filter: blur(20px) saturate(120%);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  padding: 24px;
}

.setup-modal__box {
  width: 100%;
  max-width: 640px;
  max-height: 85vh;
  background: rgba(13,17,13,0.92);
  backdrop-filter: blur(32px) saturate(160%);
  -webkit-backdrop-filter: blur(32px) saturate(160%);
  border: 1px solid rgba(92,255,166,0.14);
  border-radius: 24px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  box-shadow:
    0 0 0 1px rgba(0,0,0,0.6),
    0 40px 120px rgba(0,0,0,0.7),
    inset 0 1px 0 rgba(92,255,166,0.07);
}

.setup-modal__header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  padding: 28px 28px 20px;
  border-bottom: 1px solid rgba(92,255,166,0.07);
  flex-shrink: 0;
}

.setup-modal__eyebrow {
  font-size: 0.7rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--accent);
  margin: 0 0 6px;
}

.setup-modal__title {
  font-size: 1.5rem;
  font-weight: 800;
  color: var(--text-primary);
  margin: 0;
  letter-spacing: -0.025em;
}

.setup-modal__close {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.1);
  color: var(--text-muted);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s;
  flex-shrink: 0;
}
.setup-modal__close:hover {
  background: var(--accent);
  border-color: var(--accent);
  color: #0a0a0a;
  transform: scale(1.08);
}

/* ── Timeline ── */
.setup-modal__timeline {
  overflow-y: auto;
  padding: 24px 28px 28px;
  display: flex;
  flex-direction: column;
  gap: 0;
}

.setup-timeline__item {
  display: flex;
  gap: 20px;
  animation: fadeUp 0.4s both;
  animation-delay: var(--delay, 0s);
}

.setup-timeline__line {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex-shrink: 0;
  padding-top: 6px;
}

.setup-timeline__dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--accent);
  box-shadow: 0 0 8px rgba(92,255,166,0.5);
  flex-shrink: 0;
}

.setup-timeline__line::after {
  content: '';
  flex: 1;
  width: 1px;
  background: rgba(92,255,166,0.12);
  margin-top: 8px;
  min-height: 24px;
}
.setup-timeline__item:last-child .setup-timeline__line::after {
  display: none;
}

.setup-timeline__card {
  display: flex;
  gap: 16px;
  background: rgba(92,255,166,0.03);
  border: 1px solid rgba(92,255,166,0.08);
  border-radius: 14px;
  overflow: hidden;
  margin-bottom: 16px;
  cursor: pointer;
  flex: 1;
  transition: border-color 0.25s, background 0.25s, transform 0.25s;
}
.setup-timeline__card:hover {
  border-color: rgba(92,255,166,0.25);
  background: rgba(92,255,166,0.06);
  transform: translateX(4px);
}

.setup-timeline__media {
  position: relative;
  width: 140px;
  flex-shrink: 0;
  overflow: hidden;
}

.setup-timeline__photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.4s ease;
}
.setup-timeline__card:hover .setup-timeline__photo {
  transform: scale(1.06);
}

.setup-timeline__year-badge {
  position: absolute;
  bottom: 8px;
  left: 8px;
  font-size: 0.62rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  color: var(--accent);
  background: rgba(13,17,13,0.88);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border: 1px solid rgba(92,255,166,0.25);
  padding: 3px 8px;
  border-radius: 100px;
}

.setup-timeline__info {
  padding: 16px 16px 16px 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 6px;
}

.setup-timeline__name {
  font-size: 1rem;
  font-weight: 700;
  color: var(--text-primary);
  margin: 0;
  letter-spacing: -0.01em;
}

.setup-timeline__desc {
  font-size: 0.78rem;
  color: var(--text-muted);
  line-height: 1.55;
  font-weight: 300;
  margin: 0;
}

.setup-timeline__tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-top: 4px;
}

.setup-timeline__tag {
  font-size: 0.62rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--accent);
  background: rgba(92,255,166,0.07);
  border: 1px solid rgba(92,255,166,0.18);
  padding: 2px 8px;
  border-radius: 100px;
}

/* ── Preview full ── */
.setup-preview {
  position: fixed;
  inset: 0;
  background: rgba(2,4,2,0.92);
  backdrop-filter: blur(28px);
  -webkit-backdrop-filter: blur(28px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10000;
  padding: 24px;
}

.setup-preview__box {
  position: relative;
  width: 100%;
  max-width: 860px;
  background: rgba(13,17,13,0.95);
  border: 1px solid rgba(92,255,166,0.14);
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 40px 120px rgba(0,0,0,0.8);
}

.setup-preview__close {
  position: absolute;
  top: 14px;
  right: 14px;
  z-index: 10;
}

.setup-preview__photo {
  width: 100%;
  max-height: 520px;
  object-fit: cover;
  display: block;
}

.setup-preview__info {
  padding: 20px 24px 24px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.setup-preview__year {
  font-size: 0.7rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--accent);
}

.setup-preview__name {
  font-size: 1.4rem;
  font-weight: 700;
  color: var(--text-primary);
  margin: 0;
  letter-spacing: -0.02em;
}

.setup-preview__desc {
  font-size: 0.88rem;
  color: var(--text-muted);
  line-height: 1.65;
  font-weight: 300;
  margin: 0;
}

/* ── Transitions ── */
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}
.modal-enter-active .setup-modal__box,
.modal-enter-active .setup-preview__box {
  transition: opacity 0.3s ease, transform 0.35s cubic-bezier(0.23,1,0.32,1);
}
.modal-leave-active .setup-modal__box,
.modal-leave-active .setup-preview__box {
  transition: opacity 0.25s ease, transform 0.25s ease;
}
.modal-enter-from,
.modal-leave-to { opacity: 0; }
.modal-enter-from .setup-modal__box,
.modal-enter-from .setup-preview__box {
  opacity: 0;
  transform: scale(0.95) translateY(16px);
}
.modal-leave-to .setup-modal__box,
.modal-leave-to .setup-preview__box {
  opacity: 0;
  transform: scale(0.97) translateY(8px);
}

/* ── Animations ── */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}

@keyframes pulse {
  0%, 100% { opacity: 1; box-shadow: 0 0 8px rgba(92,255,166,0.8); }
  50%       { opacity: 0.6; box-shadow: 0 0 4px rgba(92,255,166,0.3); }
}

/* ── Responsive ── */
@media (max-width: 600px) {
  .setup-current__media { height: 220px; }
  .setup-current__specs { grid-template-columns: 1fr; }
  .setup-timeline__media { width: 100px; }
  .setup-preview__photo { max-height: 280px; }
}
</style>