<!-- MarqueeLines.vue -->
<template>
  <div class="marquee-lines">
    <!-- Línea horizontal -->
    <div class="marquee-track marquee-track--h">
      <div class="marquee-inner">
        <!-- Copia 1 -->
        <span v-for="n in 16" :key="'h1'+n" class="marquee-word">
          {{ words[n % words.length] }}<em>✦</em>
        </span>
        <!-- Copia 2 (idéntica) para el loop infinito -->
        <span v-for="n in 16" :key="'h2'+n" class="marquee-word" aria-hidden="true">
          {{ words[n % words.length] }}<em>✦</em>
        </span>
      </div>
    </div>

    <!-- Línea vertical -->
    <div class="marquee-track marquee-track--v">
      <div class="marquee-inner marquee-inner--v">
        <!-- Copia 1 -->
        <span v-for="n in 16" :key="'v1'+n" class="marquee-word">
          {{ words[n % words.length] }}<em>✦</em>
        </span>
        <!-- Copia 2 (idéntica) para el loop infinito -->
        <span v-for="n in 16" :key="'v2'+n" class="marquee-word" aria-hidden="true">
          {{ words[n % words.length] }}<em>✦</em>
        </span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const words = [
  'Keep building', 'Stay curious', 'Ship it', 'Dream bigger',
  'Code with soul', 'Make it real', 'Never stop', 'Push forward',
]
</script>

<style scoped lang="scss">
.marquee-lines {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 5;
}

.marquee-track--h {
  position: absolute;
  top: calc(50% - 15px);
  left: 0;
  width: 100%;
  height: 30px;
  overflow: hidden;
  display: flex;
  align-items: center;
}

.marquee-track--v {
  position: absolute;
  left: calc(50% - 15px);
  top: 0;
  width: 30px;
  height: 100%;
  overflow: hidden;
  display: flex;
  justify-content: center;
}

.marquee-inner {
  display: flex;
  flex-shrink: 0;
  gap: 0;
  white-space: nowrap;
  animation: scroll-h 22s linear infinite;
  will-change: transform;
}

.marquee-inner--v {
  flex-direction: column;
  white-space: normal;
  animation: scroll-v 22s linear infinite;
  writing-mode: vertical-rl;
}

.marquee-word {
  font-family: 'Courier New', monospace;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #f0f0f0;
  background: #111;
  padding: 2px 10px;
  flex-shrink: 0;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  border-right: 1px solid #333;

  em {
    font-style: normal;
    color: #3a8b04;
    font-size: 9px;
  }
}

/* Mueve exactamente la mitad (= 1 copia) para seamless loop */
@keyframes scroll-h {
  0%   { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}

@keyframes scroll-v {
  0%   { transform: translateY(0); }
  100% { transform: translateY(-50%); }
}
</style>