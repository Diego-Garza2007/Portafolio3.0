<template>
  <div class="contact-minigame">
    <!-- Grain overlay -->
    <div class="grain"></div>

    <!-- Game Title -->
    <div class="game-title">
      <span class="tag">MINI GAME</span>
      <h2>Contact Puzzle</h2>
      <p>Arrastra cada pieza a su lugar correcto</p>
    </div>

    <!-- Reset Button -->
    <button class="reset-btn" @click="resetGame">
      <Icon name="material-symbols:refresh-rounded" size="16" />
      Reset
    </button>

    <!-- Drop Zones -->
    <div class="drop-zone-container">
      <div
        v-for="zone in dropZones"
        :key="zone.id"
        v-show="!(zone.id === 'message' && placedInputs[zone.id])"
        class="drop-zone"
        :class="{ filled: placedInputs[zone.id] }"
        :style="{ left: zone.x + 'px', top: zone.y + 90 + 'px' }"
        @dragover.prevent
        @drop="handleDrop($event, zone)"
      >
        <template v-if="placedInputs[zone.id]">
          <input
            v-if="placedInputs[zone.id].type !== 'textarea'"
            :type="placedInputs[zone.id].type"
            :placeholder="placedInputs[zone.id].placeholder"
            v-model="formValues[zone.id]"
            :disabled="!gameComplete"
            class="zone-input"
          />
        </template>
        <span v-else class="drop-label">
          <Icon :name="zone.icon" size="14" />
          {{ zone.label }}
        </span>
      </div>

      <!-- Message textarea -->
      <div
        v-if="placedInputs.message"
        class="textarea-zone"
        :style="{ left: '20px', top: '235px' }"
      >
        <textarea
          :placeholder="placedInputs.message.placeholder"
          v-model="formValues.message"
          :disabled="!gameComplete"
          class="zone-textarea"
        ></textarea>
      </div>
    </div>

    <!-- Draggable Inputs -->
    <div
      v-for="input in inputs"
      :key="input.id"
      class="draggable"
      :style="{ left: input.x + 'px', top: input.y + 'px', height: input.type === 'textarea' ? '112px' : '56px' }"
      draggable="true"
      @dragstart="handleDragStart($event, input)"
    >
      <Icon :name="input.icon" size="18" class="piece-icon" />
      <span class="piece-label">{{ input.placeholder }}</span>
      <div class="drag-handle">
        <Icon name="material-symbols:drag-indicator" size="18" />
      </div>
    </div>

    <!-- Submit Button -->
    <div v-if="gameComplete" class="submit-btn-container">
      <button
        class="submit-btn"
        :disabled="!formValues.name || !formValues.email || !formValues.message"
        @click="handleSubmit"
      >
        <Icon name="material-symbols:send-rounded" size="18" />
        Enviar mensaje
      </button>
    </div>

    <!-- Success Message -->
    <div v-if="showSuccess" class="success-overlay">
      <div class="success-box">
        <div class="success-icon">
          <Icon name="material-symbols:check-circle-rounded" size="48" />
        </div>
        <h3>Mensaje enviado</h3>
        <p>Gracias por escribirme, pronto me pongo en contacto.</p>
      </div>
    </div>

    <!-- Progress Indicator -->
    <div class="progress-indicator">
      <div class="dots">
        <div
          v-for="zone in dropZones"
          :key="zone.id"
          class="dot"
          :class="{ filled: placedInputs[zone.id] }"
        ></div>
      </div>
      <p>{{ Object.keys(placedInputs).length }}/{{ dropZones.length }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

const draggedItem = ref(null);
const placedInputs = ref({});
const formValues = ref({ name: "", email: "", message: "" });
const gameComplete = ref(false);
const showSuccess = ref(false);

const dropZones = [
  { id: "name",    label: "Nombre aquí",  icon: "material-symbols:person-rounded",       x: 20, y: 20  },
  { id: "email",   label: "Email aquí",   icon: "material-symbols:alternate-email",       x: 20, y: 80  },
  { id: "message", label: "Mensaje aquí", icon: "material-symbols:chat-bubble-rounded",   x: 20, y: 140 },
];

const initialInputs = [
  { id: "name",    type: "text",     placeholder: "Tu nombre",   icon: "material-symbols:person-rounded",     x: 390, y: 50  },
  { id: "email",   type: "email",    placeholder: "Tu email",    icon: "material-symbols:alternate-email",    x: 440, y: 280 },
  { id: "message", type: "textarea", placeholder: "Tu mensaje",  icon: "material-symbols:chat-bubble-rounded",x: 530, y: 150 },
];

const inputs = ref([...initialInputs]);

watch(placedInputs, () => {
  gameComplete.value = dropZones.every(z => placedInputs.value[z.id]);
});

const handleDragStart = (e, input) => {
  draggedItem.value = input;
  e.dataTransfer.effectAllowed = "move";
};

const handleDrop = (e, zone) => {
  e.preventDefault();
  if (draggedItem.value?.id === zone.id) {
    placedInputs.value = { ...placedInputs.value, [zone.id]: draggedItem.value };
    inputs.value = inputs.value.filter(i => i.id !== draggedItem.value.id);
  }
  draggedItem.value = null;
};

const handleSubmit = () => {
  if (gameComplete.value && formValues.value.name && formValues.value.email && formValues.value.message) {
    showSuccess.value = true;
    setTimeout(() => { showSuccess.value = false; }, 3000);
  }
};

const resetGame = () => {
  inputs.value = [...initialInputs];
  placedInputs.value = {};
  formValues.value = { name: "", email: "", message: "" };
  gameComplete.value = false;
  showSuccess.value = false;
};
</script>

<style scoped lang="scss">
/* ── Tokens ── */
.contact-minigame {
  --green-900: #052e16;
  --green-800: #14532d;
  --green-700: #15803d;
  --green-600: #16a34a;
  --green-500: #22c55e;
  --green-400: #4ade80;
  --green-300: #86efac;
  --green-100: #dcfce7;
  --green-glow: rgba(34, 197, 94, 0.35);
  --surface:    rgba(5, 46, 22, 0.55);
  --border:     rgba(74, 222, 128, 0.25);
  --text-light: #f0fdf4;
  --text-muted: #86efac;
  --font: 'Inter', 'Inter fallback', sans-serif;
}

/* ── Root ── */
.contact-minigame {
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

  position: relative;
  width: 100%;
  min-height: 100%;
  background:
    radial-gradient(ellipse at 20% 20%, rgba(21, 128, 61, 0.45) 0%, transparent 55%),
    radial-gradient(ellipse at 80% 80%, rgba(5, 46, 22, 0.8) 0%, transparent 60%),
    linear-gradient(135deg, #052e16 0%, #14532d 50%, #052e16 100%);
  border-radius: 12px;
  border: 5px solid #000000;
  overflow: hidden;
  font-family: var(--font);

  /* subtle grid */
  &::before {
    content: '';
    position: absolute;
    inset: 0;
    background-image:
      linear-gradient(rgba(74,222,128,0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(74,222,128,0.04) 1px, transparent 1px);
    background-size: 32px 32px;
    pointer-events: none;
    z-index: 0;
  }

  .grain {
    position: absolute;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
    opacity: 0.5;
  }
}

/* ── Title ── */
.game-title {
  position: absolute;
  top: 16px;
  left: 18px;
  z-index: 2;

  .tag {
    display: inline-block;
    font-family: var(--font);
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.12em;
    color: var(--green-400);
    background: rgba(74, 222, 128, 0.12);
    border: 1px solid rgba(74, 222, 128, 0.3);
    border-radius: 4px;
    padding: 2px 7px;
    margin-bottom: 6px;
  }

  h2 {
    font-family: var(--font);
    font-size: 22px;
    font-weight: 700;
    color: var(--text-light);
    margin: 0 0 3px;
    letter-spacing: -0.02em;
  }

  p {
    font-family: var(--font);
    font-size: 12px;
    color: var(--text-muted);
    margin: 0;
    font-weight: 400;
  }
}

/* ── Reset ── */
.reset-btn {
  position: absolute;
  top: 16px;
  right: 16px;
  z-index: 2;
  display: flex;
  align-items: center;
  gap: 5px;
  font-family: var(--font);
  font-size: 13px;
  font-weight: 500;
  color: var(--green-300);
  background: rgba(74, 222, 128, 0.08);
  border: 1px solid rgba(74, 222, 128, 0.2);
  padding: 6px 12px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;

  &:hover {
    background: rgba(74, 222, 128, 0.16);
    border-color: rgba(74, 222, 128, 0.4);
    color: var(--green-100);
  }
}

/* ── Drop zones ── */
.drop-zone-container {
  position: absolute;
  inset: 0;
  padding: 24px;
  z-index: 1;
}

.drop-zone {
  position: absolute;
  width: 230px;
  height: 54px;
  border: 2px dashed rgba(74, 222, 128, 0.35);
  border-radius: 10px;
  background: rgba(5, 46, 22, 0.4);
  color: var(--text-muted);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.25s ease;
  backdrop-filter: blur(4px);

  .drop-label {
    font-family: var(--font);
    font-size: 13px;
    font-weight: 500;
    display: flex;
    align-items: center;
    gap: 5px;
  }

  &.filled {
    border: 2px solid var(--green-500);
    background: rgba(21, 128, 61, 0.2);
    box-shadow: 0 0 16px var(--green-glow), inset 0 0 8px rgba(34, 197, 94, 0.08);
  }

  .zone-input {
    width: 100%;
    height: 100%;
    padding: 0 12px;
    background: transparent;
    border: none;
    outline: none;
    font-family: var(--font);
    font-size: 13px;
    font-weight: 500;
    color: var(--text-light);
    border-radius: 8px;

    &::placeholder { color: var(--text-muted); }
    &:disabled { cursor: default; }
  }
}

.textarea-zone {
  position: absolute;
  width: 230px;
  height: 112px;
  border: 2px solid var(--green-500);
  border-radius: 10px;
  background: rgba(21, 128, 61, 0.2);
  box-shadow: 0 0 16px var(--green-glow), inset 0 0 8px rgba(34, 197, 94, 0.08);
  overflow: hidden;

  .zone-textarea {
    width: 100%;
    height: 100%;
    padding: 10px 12px;
    background: transparent;
    border: none;
    outline: none;
    font-family: var(--font);
    font-size: 13px;
    font-weight: 400;
    color: var(--text-light);
    resize: none;

    &::placeholder { color: var(--text-muted); }
    &:disabled { cursor: default; }
  }
}

/* ── Draggable pieces ── */
.draggable {
  position: absolute;
  width: 230px;
  background: linear-gradient(135deg, rgba(20, 83, 45, 0.9), rgba(5, 46, 22, 0.95));
  border: 1px solid rgba(74, 222, 128, 0.35);
  border-radius: 10px;
  box-shadow:
    0 4px 20px rgba(0, 0, 0, 0.4),
    0 0 0 1px rgba(74, 222, 128, 0.1) inset;
  cursor: grab;
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 0 14px;
  transition: all 0.2s;
  z-index: 3;
  backdrop-filter: blur(8px);

  .piece-icon {
    color: var(--green-400);
    flex-shrink: 0;
  }

  .piece-label {
    font-family: var(--font);
    font-size: 13px;
    font-weight: 600;
    color: var(--text-light);
    flex: 1;
  }

  .drag-handle {
    color: var(--green-600);
    display: flex;
    align-items: center;
  }

  &:active { cursor: grabbing; }

  &:hover {
    transform: translateY(-2px) scale(1.02);
    border-color: rgba(74, 222, 128, 0.6);
    box-shadow:
      0 8px 28px rgba(0, 0, 0, 0.5),
      0 0 20px var(--green-glow);
  }
}

/* ── Submit ── */
.submit-btn-container {
  position: absolute;
  bottom: 18px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 4;
}

.submit-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 11px 24px;
  font-family: var(--font);
  font-size: 14px;
  font-weight: 600;
  color: var(--green-900);
  background: linear-gradient(135deg, var(--green-400), var(--green-500));
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.2s;
  box-shadow: 0 0 20px var(--green-glow);
  letter-spacing: 0.01em;

  &:disabled {
    background: rgba(74, 222, 128, 0.2);
    color: var(--green-700);
    box-shadow: none;
    cursor: not-allowed;
  }

  &:hover:not(:disabled) {
    background: linear-gradient(135deg, var(--green-300), var(--green-400));
    transform: translateY(-1px);
    box-shadow: 0 4px 28px var(--green-glow);
  }
}

/* ── Success ── */
.success-overlay {
  position: absolute;
  inset: 0;
  background: rgba(5, 46, 22, 0.75);
  backdrop-filter: blur(6px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;

  .success-box {
    background: linear-gradient(135deg, rgba(20, 83, 45, 0.95), rgba(5, 46, 22, 0.98));
    border: 1px solid rgba(74, 222, 128, 0.4);
    padding: 32px 40px;
    border-radius: 16px;
    text-align: center;
    box-shadow:
      0 20px 60px rgba(0, 0, 0, 0.5),
      0 0 40px var(--green-glow);
    animation: pop 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);

    .success-icon {
      color: var(--green-400);
      margin-bottom: 12px;
    }

    h3 {
      font-family: var(--font);
      font-size: 20px;
      font-weight: 700;
      color: var(--green-300);
      margin: 0 0 6px;
      letter-spacing: -0.01em;
    }

    p {
      font-family: var(--font);
      font-size: 13px;
      color: var(--text-muted);
      margin: 0;
    }
  }
}

@keyframes pop {
  from { opacity: 0; transform: scale(0.8); }
  to   { opacity: 1; transform: scale(1); }
}

/* ── Progress ── */
.progress-indicator {
  position: absolute;
  bottom: 18px;
  right: 16px;
  background: rgba(5, 46, 22, 0.6);
  border: 1px solid var(--border);
  padding: 7px 10px;
  border-radius: 8px;
  text-align: center;
  z-index: 2;
  backdrop-filter: blur(4px);

  .dots {
    display: flex;
    gap: 5px;
    justify-content: center;
    margin-bottom: 4px;

    .dot {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: rgba(74, 222, 128, 0.2);
      border: 1px solid rgba(74, 222, 128, 0.25);
      transition: all 0.3s;

      &.filled {
        background: var(--green-400);
        box-shadow: 0 0 6px var(--green-glow);
        border-color: var(--green-400);
      }
    }
  }

  p {
    font-family: var(--font);
    color: var(--text-muted);
    font-size: 11px;
    font-weight: 600;
    margin: 0;
  }
}
</style>