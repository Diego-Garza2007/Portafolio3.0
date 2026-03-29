<template>
  <div class="box">
    <!-- Slot de fondo (ej. CaveParallax) — tiene prioridad sobre el video -->
    <div v-if="$slots.background" class="box__background">
      <slot name="background" />
    </div>

    <!-- Video de fondo opcional (solo si no hay slot background) -->
    <video v-else-if="videoSrc" autoplay muted loop class="box__video">
      <source :src="videoSrc" type="video/mp4" />
      Tu navegador no soporta videos.
    </video>

    <!-- Label encima -->
    <div class="box__content" :class="{ 'box__content--right': alignRight }">
      <slot />
    </div>
  </div>
</template>

<script setup>
defineProps({
  videoSrc: {
    type: String,
    required: false,
    default: "",
  },
  alignRight: {
    type: Boolean,
    required: false,
    default: false,
  },
});
</script>

<style scoped lang="scss">
.box {
  position: relative;
  width: 100%;
  height: 100%;
  border: 5px solid black;
  border-radius: 15px;
  overflow: hidden;
  background-color: #0a0a0f;
  transition: transform 0.3s ease;
}

.box__background {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}

.box__video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: 0;
}

.box__content {
  position: absolute;
  top: 10px;
  left: 10px;
  z-index: 1;
  color: white;
  font-weight: bold;
  font-size: 2rem;
  padding: 10px 20px;
  border-radius: 10px;
  background: rgba(0, 0, 0, 0.288);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(10px);
}

.box__content--right {
  left: auto;
  right: 10px;
}
</style>