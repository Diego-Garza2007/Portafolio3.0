<template>
  <div
    class="cave"
    ref="cave"
    @mousemove="handleMouseMove"
    @mouseleave="resetParallax"
  >
    <svg
      class="cave__svg"
      viewBox="0 0 800 500"
      xmlns="http://www.w3.org/2000/svg"
      preserveAspectRatio="xMidYMid slice"
    >
      <defs>
        <linearGradient id="sky" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%"   stop-color="#0b0b2e" />
          <stop offset="40%"  stop-color="#131650" />
          <stop offset="100%" stop-color="#1a1060" />
        </linearGradient>
        <radialGradient id="glow-purple" cx="50%" cy="50%" r="50%">
          <stop offset="0%"   stop-color="#7c3aed" stop-opacity="0.9" />
          <stop offset="100%" stop-color="#7c3aed" stop-opacity="0" />
        </radialGradient>
        <radialGradient id="glow-blue" cx="50%" cy="50%" r="50%">
          <stop offset="0%"   stop-color="#3b82f6" stop-opacity="0.8" />
          <stop offset="100%" stop-color="#3b82f6" stop-opacity="0" />
        </radialGradient>
        <linearGradient id="fog" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%"   stop-color="#1e1b6e" stop-opacity="0" />
          <stop offset="100%" stop-color="#1e1b6e" stop-opacity="0.55" />
        </linearGradient>
      </defs>

      <!-- Sky -->
      <rect width="800" height="500" fill="url(#sky)" />

      <!-- Far glow hazes -->
      <ellipse class="cave__layer" data-depth="0.04" cx="200" cy="320" rx="180" ry="90" fill="#4c1d95" opacity="0.25" />
      <ellipse class="cave__layer" data-depth="0.04" cx="600" cy="300" rx="200" ry="100" fill="#1d4ed8" opacity="0.2" />
      <ellipse class="cave__layer" data-depth="0.04" cx="400" cy="350" rx="220" ry="80" fill="#5b21b6" opacity="0.18" />

      <!-- Layer 1: far buildings -->
      <g class="cave__layer" data-depth="0.06" opacity="0.45">
        <rect x="30"  y="230" width="28" height="160" fill="#0f0e2a" />
        <rect x="22"  y="210" width="12" height="20"  fill="#0f0e2a" />
        <rect x="70"  y="245" width="22" height="145" fill="#0f0e2a" />
        <rect x="120" y="220" width="32" height="170" fill="#0f0e2a" />
        <rect x="128" y="200" width="14" height="22"  fill="#0f0e2a" />
        <rect x="165" y="250" width="20" height="140" fill="#0f0e2a" />
        <rect x="580" y="235" width="30" height="155" fill="#0f0e2a" />
        <rect x="586" y="215" width="14" height="22"  fill="#0f0e2a" />
        <rect x="620" y="255" width="24" height="135" fill="#0f0e2a" />
        <rect x="660" y="228" width="34" height="162" fill="#0f0e2a" />
        <rect x="668" y="205" width="16" height="25"  fill="#0f0e2a" />
        <rect x="706" y="248" width="22" height="142" fill="#0f0e2a" />
        <rect x="740" y="238" width="28" height="152" fill="#0f0e2a" />
      </g>

      <!-- Layer 2: mid buildings with windows -->
      <g class="cave__layer" data-depth="0.13" opacity="0.75">
        <rect x="-10" y="160" width="55" height="230" fill="#100f30" />
        <rect x="3"   y="138" width="22" height="24"  fill="#100f30" />
        <rect x="2"   y="170" width="10" height="14" fill="#7c3aed" opacity="0.9" />
        <rect x="18"  y="170" width="10" height="14" fill="#4f46e5" opacity="0.7" />
        <rect x="2"   y="195" width="10" height="14" fill="#4f46e5" opacity="0.5" />
        <rect x="18"  y="195" width="10" height="14" fill="#7c3aed" opacity="0.8" />
        <rect x="2"   y="220" width="10" height="14" fill="#7c3aed" opacity="0.6" />
        <rect x="18"  y="220" width="10" height="14" fill="#4f46e5" opacity="0.9" />
        <rect x="55"  y="185" width="42" height="205" fill="#12103a" />
        <rect x="63"  y="162" width="18" height="25"  fill="#12103a" />
        <rect x="58"  y="195" width="10" height="14" fill="#3b82f6" opacity="0.8" />
        <rect x="74"  y="195" width="10" height="14" fill="#7c3aed" opacity="0.7" />
        <rect x="58"  y="220" width="10" height="14" fill="#7c3aed" opacity="0.9" />
        <rect x="74"  y="220" width="10" height="14" fill="#3b82f6" opacity="0.6" />
        <rect x="58"  y="245" width="10" height="14" fill="#4f46e5" opacity="0.8" />
        <rect x="74"  y="245" width="10" height="14" fill="#7c3aed" opacity="0.5" />
        <rect x="220" y="175" width="48" height="215" fill="#0e0d2e" />
        <rect x="230" y="150" width="20" height="27"  fill="#0e0d2e" />
        <rect x="223" y="185" width="12" height="16" fill="#7c3aed" opacity="0.9" />
        <rect x="242" y="185" width="12" height="16" fill="#3b82f6" opacity="0.7" />
        <rect x="223" y="212" width="12" height="16" fill="#4f46e5" opacity="0.8" />
        <rect x="242" y="212" width="12" height="16" fill="#7c3aed" opacity="0.6" />
        <rect x="223" y="239" width="12" height="16" fill="#7c3aed" opacity="0.7" />
        <rect x="242" y="239" width="12" height="16" fill="#3b82f6" opacity="0.9" />
        <rect x="530" y="168" width="52" height="222" fill="#100f30" />
        <rect x="542" y="143" width="22" height="27"  fill="#100f30" />
        <rect x="534" y="178" width="12" height="16" fill="#3b82f6" opacity="0.85" />
        <rect x="553" y="178" width="12" height="16" fill="#7c3aed" opacity="0.7" />
        <rect x="534" y="205" width="12" height="16" fill="#7c3aed" opacity="0.9" />
        <rect x="553" y="205" width="12" height="16" fill="#4f46e5" opacity="0.65" />
        <rect x="534" y="232" width="12" height="16" fill="#4f46e5" opacity="0.8" />
        <rect x="553" y="232" width="12" height="16" fill="#7c3aed" opacity="0.75" />
        <rect x="700" y="172" width="50" height="218" fill="#12103a" />
        <rect x="712" y="148" width="20" height="26"  fill="#12103a" />
        <rect x="703" y="182" width="12" height="16" fill="#7c3aed" opacity="0.9" />
        <rect x="722" y="182" width="12" height="16" fill="#3b82f6" opacity="0.7" />
        <rect x="703" y="209" width="12" height="16" fill="#4f46e5" opacity="0.8" />
        <rect x="722" y="209" width="12" height="16" fill="#7c3aed" opacity="0.6" />
        <rect x="703" y="236" width="12" height="16" fill="#7c3aed" opacity="0.7" />
        <rect x="722" y="236" width="12" height="16" fill="#4f46e5" opacity="0.9" />
        <rect x="758" y="190" width="44" height="200" fill="#100f30" />
        <rect x="767" y="168" width="18" height="24"  fill="#100f30" />
        <rect x="761" y="200" width="10" height="14" fill="#3b82f6" opacity="0.8" />
        <rect x="778" y="200" width="10" height="14" fill="#7c3aed" opacity="0.75" />
        <rect x="761" y="225" width="10" height="14" fill="#7c3aed" opacity="0.9" />
        <rect x="778" y="225" width="10" height="14" fill="#4f46e5" opacity="0.6" />
      </g>

      <!-- Window glow halos -->
      <g class="cave__layer" data-depth="0.13" opacity="0.4">
        <ellipse cx="15"  cy="200" rx="30" ry="20" fill="url(#glow-purple)" />
        <ellipse cx="76"  cy="215" rx="28" ry="18" fill="url(#glow-blue)" />
        <ellipse cx="246" cy="210" rx="32" ry="20" fill="url(#glow-purple)" />
        <ellipse cx="553" cy="205" rx="32" ry="20" fill="url(#glow-blue)" />
        <ellipse cx="718" cy="208" rx="30" ry="18" fill="url(#glow-purple)" />
      </g>

      <!-- Layer 3: main close buildings -->
      <g class="cave__layer" data-depth="0.22" opacity="0.92">
        <rect x="-20" y="80"  width="80" height="340" fill="#0a0924" />
        <rect x="8"   y="52"  width="28" height="32"  fill="#0a0924" />
        <rect x="18"  y="38"  width="8"  height="18"  fill="#0a0924" />
        <rect x="-4"  y="100" width="18" height="22" fill="#7c3aed" opacity="0.95" />
        <rect x="22"  y="100" width="18" height="22" fill="#4f46e5" opacity="0.85" />
        <rect x="44"  y="100" width="18" height="22" fill="#7c3aed" opacity="0.7" />
        <rect x="-4"  y="138" width="18" height="22" fill="#3b82f6" opacity="0.9" />
        <rect x="22"  y="138" width="18" height="22" fill="#7c3aed" opacity="0.95" />
        <rect x="44"  y="138" width="18" height="22" fill="#4f46e5" opacity="0.8" />
        <rect x="-4"  y="176" width="18" height="22" fill="#7c3aed" opacity="0.7" />
        <rect x="22"  y="176" width="18" height="22" fill="#3b82f6" opacity="0.85" />
        <rect x="44"  y="176" width="18" height="22" fill="#7c3aed" opacity="0.9" />
        <rect x="-4"  y="214" width="18" height="22" fill="#4f46e5" opacity="0.95" />
        <rect x="22"  y="214" width="18" height="22" fill="#7c3aed" opacity="0.75" />
        <rect x="44"  y="214" width="18" height="22" fill="#3b82f6" opacity="0.9" />
        <rect x="72"  y="130" width="65" height="290" fill="#0c0b28" />
        <rect x="90"  y="104" width="26" height="30"  fill="#0c0b28" />
        <rect x="99"  y="88"  width="8"  height="20"  fill="#0c0b28" />
        <rect x="76"  y="148" width="14" height="18" fill="#7c3aed" opacity="0.9" />
        <rect x="96"  y="148" width="14" height="18" fill="#4f46e5" opacity="0.8" />
        <rect x="116" y="148" width="14" height="18" fill="#3b82f6" opacity="0.85" />
        <rect x="76"  y="178" width="14" height="18" fill="#4f46e5" opacity="0.75" />
        <rect x="96"  y="178" width="14" height="18" fill="#7c3aed" opacity="0.95" />
        <rect x="116" y="178" width="14" height="18" fill="#4f46e5" opacity="0.7" />
        <rect x="76"  y="208" width="14" height="18" fill="#7c3aed" opacity="0.85" />
        <rect x="96"  y="208" width="14" height="18" fill="#3b82f6" opacity="0.9" />
        <rect x="116" y="208" width="14" height="18" fill="#7c3aed" opacity="0.8" />
        <rect x="740" y="90"  width="78" height="330" fill="#0a0924" />
        <rect x="754" y="62"  width="28" height="32"  fill="#0a0924" />
        <rect x="762" y="46"  width="8"  height="20"  fill="#0a0924" />
        <rect x="744" y="110" width="18" height="22" fill="#7c3aed" opacity="0.9" />
        <rect x="770" y="110" width="18" height="22" fill="#3b82f6" opacity="0.85" />
        <rect x="796" y="110" width="18" height="22" fill="#7c3aed" opacity="0.7" />
        <rect x="744" y="148" width="18" height="22" fill="#4f46e5" opacity="0.95" />
        <rect x="770" y="148" width="18" height="22" fill="#7c3aed" opacity="0.8" />
        <rect x="796" y="148" width="18" height="22" fill="#3b82f6" opacity="0.9" />
        <rect x="744" y="186" width="18" height="22" fill="#7c3aed" opacity="0.75" />
        <rect x="770" y="186" width="18" height="22" fill="#4f46e5" opacity="0.9" />
        <rect x="796" y="186" width="18" height="22" fill="#7c3aed" opacity="0.85" />
        <rect x="660" y="140" width="68" height="280" fill="#0c0b28" />
        <rect x="678" y="112" width="28" height="32"  fill="#0c0b28" />
        <rect x="688" y="96"  width="8"  height="20"  fill="#0c0b28" />
        <rect x="664" y="158" width="14" height="18" fill="#7c3aed" opacity="0.9" />
        <rect x="684" y="158" width="14" height="18" fill="#4f46e5" opacity="0.8" />
        <rect x="704" y="158" width="14" height="18" fill="#3b82f6" opacity="0.85" />
        <rect x="664" y="188" width="14" height="18" fill="#4f46e5" opacity="0.75" />
        <rect x="684" y="188" width="14" height="18" fill="#7c3aed" opacity="0.95" />
        <rect x="704" y="188" width="14" height="18" fill="#4f46e5" opacity="0.7" />
        <rect x="664" y="218" width="14" height="18" fill="#7c3aed" opacity="0.85" />
        <rect x="684" y="218" width="14" height="18" fill="#3b82f6" opacity="0.9" />
        <rect x="704" y="218" width="14" height="18" fill="#7c3aed" opacity="0.8" />
      </g>

      <!-- Cave arch top -->
      <path
        class="cave__layer"
        data-depth="0.3"
        d="M0,0 L0,200 Q60,130 130,168 Q200,205 280,155 Q360,105 440,158 Q520,210 600,162 Q680,114 760,170 Q800,195 800,190 L800,0 Z"
        fill="#06050f"
      />

      <!-- Cave arch bottom -->
      <path
        class="cave__layer"
        data-depth="0.3"
        d="M0,500 L0,350 Q70,390 150,345 Q230,300 310,352 Q390,402 470,348 Q550,295 630,350 Q710,400 800,342 L800,500 Z"
        fill="#06050f"
      />

      <!-- Stalactites foreground -->
      <g class="cave__layer" data-depth="0.38">
        <polygon points="-10,0 18,220 -38,220"   fill="#04040d" />
        <polygon points="105,0 136,195 74,195"   fill="#04040d" />
        <polygon points="235,0 262,210 208,210"  fill="#04040d" />
        <polygon points="375,0 400,200 350,200"  fill="#04040d" />
        <polygon points="510,0 538,215 482,215"  fill="#04040d" />
        <polygon points="645,0 672,205 618,205"  fill="#04040d" />
        <polygon points="770,0 798,198 742,198"  fill="#04040d" />
      </g>

      <!-- Stalagmites foreground -->
      <g class="cave__layer" data-depth="0.38">
        <polygon points="-10,500 16,300 -36,300"  fill="#04040d" />
        <polygon points="110,500 138,310 82,310"  fill="#04040d" />
        <polygon points="250,500 276,295 224,295" fill="#04040d" />
        <polygon points="400,500 424,308 376,308" fill="#04040d" />
        <polygon points="540,500 566,300 514,300" fill="#04040d" />
        <polygon points="678,500 704,305 652,305" fill="#04040d" />
        <polygon points="810,500 836,298 784,298" fill="#04040d" />
      </g>

      <!-- Rain drops -->
      <g class="cave__layer" data-depth="0.15">
        <line class="drop"    x1="80"  y1="0"   x2="76"  y2="22"  stroke="#93c5fd" stroke-width="1.2" opacity="0.55" />
        <line class="drop"    x1="145" y1="30"  x2="141" y2="52"  stroke="#93c5fd" stroke-width="1"   opacity="0.45" />
        <line class="drop"    x1="200" y1="10"  x2="196" y2="32"  stroke="#a5b4fc" stroke-width="1.2" opacity="0.6"  />
        <line class="drop"    x1="270" y1="50"  x2="266" y2="72"  stroke="#93c5fd" stroke-width="1"   opacity="0.5"  />
        <line class="drop"    x1="330" y1="20"  x2="326" y2="42"  stroke="#93c5fd" stroke-width="1.2" opacity="0.55" />
        <line class="drop"    x1="395" y1="5"   x2="391" y2="27"  stroke="#a5b4fc" stroke-width="1"   opacity="0.45" />
        <line class="drop"    x1="450" y1="40"  x2="446" y2="62"  stroke="#93c5fd" stroke-width="1.2" opacity="0.6"  />
        <line class="drop"    x1="510" y1="15"  x2="506" y2="37"  stroke="#93c5fd" stroke-width="1"   opacity="0.5"  />
        <line class="drop"    x1="575" y1="35"  x2="571" y2="57"  stroke="#a5b4fc" stroke-width="1.2" opacity="0.55" />
        <line class="drop"    x1="635" y1="8"   x2="631" y2="30"  stroke="#93c5fd" stroke-width="1"   opacity="0.45" />
        <line class="drop"    x1="690" y1="45"  x2="686" y2="67"  stroke="#93c5fd" stroke-width="1.2" opacity="0.6"  />
        <line class="drop"    x1="750" y1="22"  x2="746" y2="44"  stroke="#a5b4fc" stroke-width="1"   opacity="0.5"  />
        <line class="drop d2" x1="110" y1="80"  x2="106" y2="102" stroke="#93c5fd" stroke-width="1"   opacity="0.4"  />
        <line class="drop d2" x1="175" y1="120" x2="171" y2="142" stroke="#a5b4fc" stroke-width="1.2" opacity="0.5"  />
        <line class="drop d2" x1="240" y1="90"  x2="236" y2="112" stroke="#93c5fd" stroke-width="1"   opacity="0.45" />
        <line class="drop d2" x1="305" y1="140" x2="301" y2="162" stroke="#93c5fd" stroke-width="1.2" opacity="0.55" />
        <line class="drop d2" x1="360" y1="100" x2="356" y2="122" stroke="#a5b4fc" stroke-width="1"   opacity="0.4"  />
        <line class="drop d2" x1="425" y1="130" x2="421" y2="152" stroke="#93c5fd" stroke-width="1.2" opacity="0.5"  />
        <line class="drop d2" x1="480" y1="85"  x2="476" y2="107" stroke="#93c5fd" stroke-width="1"   opacity="0.45" />
        <line class="drop d2" x1="545" y1="115" x2="541" y2="137" stroke="#a5b4fc" stroke-width="1.2" opacity="0.55" />
        <line class="drop d2" x1="605" y1="75"  x2="601" y2="97"  stroke="#93c5fd" stroke-width="1"   opacity="0.4"  />
        <line class="drop d2" x1="665" y1="105" x2="661" y2="127" stroke="#93c5fd" stroke-width="1.2" opacity="0.5"  />
        <line class="drop d2" x1="720" y1="90"  x2="716" y2="112" stroke="#a5b4fc" stroke-width="1"   opacity="0.45" />
        <line class="drop d2" x1="780" y1="60"  x2="776" y2="82"  stroke="#93c5fd" stroke-width="1.2" opacity="0.55" />
        <line class="drop d3" x1="55"  y1="160" x2="51"  y2="182" stroke="#93c5fd" stroke-width="1"   opacity="0.35" />
        <line class="drop d3" x1="160" y1="200" x2="156" y2="222" stroke="#a5b4fc" stroke-width="1.2" opacity="0.45" />
        <line class="drop d3" x1="290" y1="180" x2="286" y2="202" stroke="#93c5fd" stroke-width="1"   opacity="0.4"  />
        <line class="drop d3" x1="410" y1="220" x2="406" y2="242" stroke="#93c5fd" stroke-width="1.2" opacity="0.5"  />
        <line class="drop d3" x1="490" y1="170" x2="486" y2="192" stroke="#a5b4fc" stroke-width="1"   opacity="0.35" />
        <line class="drop d3" x1="560" y1="210" x2="556" y2="232" stroke="#93c5fd" stroke-width="1.2" opacity="0.45" />
        <line class="drop d3" x1="620" y1="185" x2="616" y2="207" stroke="#93c5fd" stroke-width="1"   opacity="0.4"  />
        <line class="drop d3" x1="700" y1="155" x2="696" y2="177" stroke="#a5b4fc" stroke-width="1.2" opacity="0.5"  />
        <line class="drop d3" x1="760" y1="195" x2="756" y2="217" stroke="#93c5fd" stroke-width="1"   opacity="0.35" />
      </g>

      <!-- Soul wisps -->
      <g class="cave__layer" data-depth="0.1">
        <circle class="wsp"  cx="190" cy="255" r="4"   fill="#a78bfa" opacity="0.85" />
        <circle class="wsp"  cx="190" cy="255" r="10"  fill="#7c3aed" opacity="0.2"  />
        <circle class="wsp2" cx="360" cy="220" r="3"   fill="#c4b5fd" opacity="0.7"  />
        <circle class="wsp2" cx="360" cy="220" r="8"   fill="#6d28d9" opacity="0.18" />
        <circle class="wsp3" cx="500" cy="268" r="4.5" fill="#818cf8" opacity="0.8"  />
        <circle class="wsp3" cx="500" cy="268" r="12"  fill="#4f46e5" opacity="0.15" />
        <circle class="wsp"  cx="620" cy="238" r="3"   fill="#a78bfa" opacity="0.75" />
        <circle class="wsp"  cx="620" cy="238" r="8"   fill="#7c3aed" opacity="0.18" />
        <circle class="wsp2" cx="280" cy="300" r="3.5" fill="#ddd6fe" opacity="0.6"  />
        <circle class="wsp3" cx="680" cy="285" r="4"   fill="#c4b5fd" opacity="0.7"  />
      </g>

      <!-- Fog overlay -->
      <rect class="cave__layer" data-depth="0.05" width="800" height="500" fill="url(#fog)" />

      <!-- Knight silhouette -->
      <g class="cave__knight" transform="translate(376, 222)">
        <ellipse cx="18" cy="80" rx="22" ry="6" fill="#000" opacity="0.4" />
        <ellipse cx="18" cy="52" rx="22" ry="30" fill="#08080f" />
        <rect x="6"  y="20" width="24" height="28" rx="3" fill="#08080f" />
        <rect x="4"  y="3"  width="28" height="22" rx="5" fill="#08080f" />
        <line x1="8"  y1="3"  x2="0"  y2="-12" stroke="#08080f" stroke-width="5" stroke-linecap="round" />
        <line x1="28" y1="3"  x2="36" y2="-12" stroke="#08080f" stroke-width="5" stroke-linecap="round" />
        <circle cx="11" cy="14" r="4" fill="#f5f0ff" />
        <circle cx="25" cy="14" r="4" fill="#f5f0ff" />
        <circle cx="18" cy="36" r="5"  fill="#818cf8" opacity="0.9" />
        <circle cx="18" cy="36" r="10" fill="#6d28d9" opacity="0.25" />
        <line x1="32" y1="38" x2="68" y2="25" stroke="#d4c8f0" stroke-width="3.5" stroke-linecap="round" />
        <line x1="42" y1="35" x2="46" y2="44" stroke="#9ca3af"  stroke-width="2.5" stroke-linecap="round" />
      </g>

      <!-- Play label -->
      <text
        x="400" y="470"
        text-anchor="middle"
        font-family="'Inter', sans-serif"
        font-size="12"
        font-weight="700"
        letter-spacing="0.2em"
        fill="#c4b5fd"
        opacity="0.8"
      >PLAY</text>
    </svg>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const cave = ref(null)
let layers = []
let animFrame = null
let targetX = 0
let targetY = 0
let currentX = 0
let currentY = 0

onMounted(() => {
  layers = cave.value.querySelectorAll('.cave__layer')
  animFrame = requestAnimationFrame(tick)
})

onUnmounted(() => {
  if (animFrame) cancelAnimationFrame(animFrame)
})

const handleMouseMove = (e) => {
  const rect = cave.value.getBoundingClientRect()
  targetX = (e.clientX - rect.left - rect.width  / 2) / rect.width
  targetY = (e.clientY - rect.top  - rect.height / 2) / rect.height
}

const resetParallax = () => {
  targetX = 0
  targetY = 0
}

const tick = () => {
  currentX += (targetX - currentX) * 0.055
  currentY += (targetY - currentY) * 0.055

  layers.forEach((el) => {
    const depth = parseFloat(el.dataset.depth || 0.1)
    const dx = currentX * depth * 120
    const dy = currentY * depth * 80
    el.style.transform = `translate(${dx}px, ${dy}px)`
  })

  animFrame = requestAnimationFrame(tick)
}
</script>

<style scoped lang="scss">
.cave {
  position: relative;
  width: 100%;
  height: 100%;
  min-height: 260px;
  overflow: hidden;
  cursor: crosshair;
  background: #0b0b2e;

  &__svg {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
  }

  &__layer {
    will-change: transform;
  }

  &__knight {
    pointer-events: none;
  }
}

.drop {
  animation: fall 1.4s linear infinite;
  &.d2 { animation-duration: 1.1s; animation-delay: 0.45s; }
  &.d3 { animation-duration: 1.6s; animation-delay: 0.9s;  }
}

@keyframes fall {
  0%   { transform: translateY(-20px); opacity: 0;   }
  15%  { opacity: 1; }
  85%  { opacity: 1; }
  100% { transform: translateY(520px); opacity: 0; }
}

.wsp  { animation: wisp 3.6s ease-in-out infinite alternate; }
.wsp2 { animation: wisp 4.3s ease-in-out infinite alternate; animation-delay: 1.2s; }
.wsp3 { animation: wisp 3.0s ease-in-out infinite alternate; animation-delay: 0.6s; }

@keyframes wisp {
  from { transform: translateY(0);     opacity: 0.85; }
  to   { transform: translateY(-12px); opacity: 0.2;  }
}
</style>