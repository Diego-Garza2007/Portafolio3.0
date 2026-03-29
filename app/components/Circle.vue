<template>
  <div class="circle-wrapper">
    <!-- Fondo negro semitransparente sobre la imagen -->
    <div class="circle">
      <div class="circle-overlay"></div>
      <!-- Texto girando alrededor del círculo -->
      <svg viewBox="0 0 300 300" class="circle-text rotating">
        <defs>
          <path
            id="circlePath"
            d="M150,150 m-130,0 a130,130 0 1,1 260,0 a130,130 0 1,1 -260,0"
          />
          <path
            id="circlePathBottom"
            d="M150,150 m130,0 a130,130 0 1,0 -260,0 a130,130 0 1,0 260,0"
          />
        </defs>
        <text fill="#fff" font-size="18" font-weight="bold">
          <textPath href="#circlePath" startOffset="8%" text-anchor="start" class="top-text">
            Diego Emilio Garza Gudino
          </textPath>
        </text>
        <text font-size="16" font-weight="bold">
          <textPath href="#circlePathBottom" startOffset="62%" text-anchor="start" class="bottom-text">
            Front End Developer
          </textPath>
        </text>
      </svg>
    </div>
    <!-- Líneas y círculo blanco de fondo -->
    <div class="circle-background"></div>
    <div class="line line-south"></div>
    <div class="line line-north"></div>
    <!-- ✦ NUEVO: texto orbitando por fuera del círculo blanco grande -->
    <svg viewBox="0 0 500 500" class="orbit-text">
      <defs>
        <path
          id="orbitPath"
          d="M250,250 m-170,0 a170,170 0 1,1 340,0 a170,170 0 1,1 -340,0"
        />
      </defs>
      <text font-size="16" font-weight="700" letter-spacing="3" font-family="'Courier New', monospace" fill="#111">
        <textPath href="#orbitPath" startOffset="0%">
          ✦ KEEP BUILDING ✦ STAY CURIOUS ✦ DREAM BIGGER ✦ MAKE IT REAL ✦ NEVER STOP ✦ VUE FAN
        </textPath>
      </text>
    </svg>
  </div>
</template>

<script setup lang="ts"></script>

<style scoped lang="scss">
.circle-wrapper {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 500px;
  height: 500px;
  transform: translate(-50%, -50%);
  z-index: 20;
  pointer-events: none;
}

.circle {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 300px;
  height: 300px;
  transform: translate(-50%, -50%);
  border-radius: 50%;
  background-image: url('test2q.png');
  background-size: cover;
  background-position: center 10%;
  background-repeat: no-repeat;
  overflow: hidden;
  z-index: 20;

  .circle-overlay {
    position: absolute;
    inset: 0;
    background-color: rgba(0, 0, 0, 0.4);
    border-radius: 50%;
    z-index: 10;
  }

  .circle-text {
    position: absolute;
    top: 0px;
    left: 0px;
    width: 100%;
    height: 100%;
    z-index: 20;
    transform-origin: center;

    :deep(.top-text) {
      fill: #ffffff;
      letter-spacing: 0.5px;
      text-transform: uppercase;
    }

    :deep(.bottom-text) {
      fill: hsl(100, 90%, 45%);
      letter-spacing: 0.5px;
      text-transform: uppercase;
    }
  }
}

.circle-background {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 400px;
  height: 400px;
  transform: translate(-50%, -50%);
  background-color: #f0ede8;
  border-radius: 50%;
  border: 5px solid #222;
  z-index: 10;
  pointer-events: auto;
}

.line {
  position: absolute;
  background-color: #f0ede8;
  z-index: 15;
  pointer-events: none;
}

.line-south {
  width: 30px;
  height: 100%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.line-north {
  width: 100%;
  height: 30px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.orbit-text {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 25;
  transform-origin: center;
  animation: spin-orbit 25s linear infinite;
  pointer-events: none;
}

@keyframes spin-orbit {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}
</style>