<template>
  <div class="game-container">
    <!-- HUD Superior -->
    <div class="hud">
      <NuxtLink to="/" class="back-btn">
        <Icon name="material-symbols:arrow-back-rounded" size="18" />
        Inicio
      </NuxtLink>
      <div class="score-display">
        <Icon name="material-symbols:star-rounded" size="24" />
        <span class="score">{{ score }}</span>
      </div>
      <div class="lives-display">
        <Icon 
          v-for="i in maxLives" 
          :key="i"
          name="material-symbols:favorite-rounded" 
          size="20"
          :class="{ 'life-lost': i > lives }"
        />
      </div>
    </div>

    <!-- Canvas del Juego -->
    <div ref="gameCanvas" class="game-area"></div>

    <!-- Pantalla de Victoria -->
    <div v-if="showVictory" class="victory-overlay">
      <div class="victory-card">
        <Icon name="material-symbols:trophy" size="80" class="trophy-icon" />
        <h2>¡Felicidades!</h2>
        <p>Has alcanzado {{ targetScore }} puntos</p>
        <button @click="openCV" class="cv-button">
          <Icon name="material-symbols:picture-as-pdf" size="24" />
          Ver mi CV
        </button>
        <button @click="restartGame" class="restart-button">
          Jugar de nuevo
        </button>
      </div>
    </div>

    <!-- Game Over -->
    <div v-if="gameOver && !showVictory" class="gameover-overlay">
      <div class="gameover-card">
        <h2>Game Over</h2>
        <p>Puntuación: {{ score }}</p>
        <p class="target-info">Necesitas {{ targetScore }} puntos para desbloquear el CV</p>
        <button @click="restartGame" class="restart-button">
          Intentar de nuevo
        </button>
      </div>
    </div>

    <!-- Instrucciones -->
    <div class="instructions">
      <span><Icon name="material-symbols:keyboard-arrow-left-rounded" size="16" /> <Icon name="material-symbols:keyboard-arrow-right-rounded" size="16" /> Mover</span>
      <span>• Atrapa las estrellas ⭐</span>
      <span>• Evita las bombas 💣</span>
      <span>• Alcanza {{ targetScore }} puntos</span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const gameCanvas = ref(null)
const score = ref(0)
const lives = ref(3)
const maxLives = ref(3)
const gameOver = ref(false)
const showVictory = ref(false)
const targetScore = 5 // Puntuación objetivo para abrir el CV

let game = null

// Función para abrir el CV
const openCV = () => {
  window.open('/CVDiegoGarza.pdf', '_blank')
}

const restartGame = () => {
  score.value = 0
  lives.value = 3
  gameOver.value = false
  showVictory.value = false
  if (game && game.scene.scenes[0]) {
    game.scene.scenes[0].scene.restart()
  }
}

onMounted(async () => {
  // Cargar Phaser dinámicamente
  const Phaser = await import('https://cdn.jsdelivr.net/npm/phaser@3.80.1/dist/phaser.esm.js')
  const P = Phaser.default ?? Phaser

  class GameScene extends P.Scene {
    constructor() {
      super({ key: 'GameScene' })
    }

    create() {
      const W = this.scale.width
      const H = this.scale.height

      // Fondo con gradiente
      const graphics = this.add.graphics()
      graphics.fillGradientStyle(0x1e1b4b, 0x1e1b4b, 0x0f172a, 0x0f172a, 1)
      graphics.fillRect(0, 0, W, H)

      // Estrellas de fondo
      for (let i = 0; i < 50; i++) {
        const star = this.add.circle(
          Phaser.Math.Between(0, W),
          Phaser.Math.Between(0, H),
          Phaser.Math.Between(1, 2),
          0xffffff,
          Phaser.Math.FloatBetween(0.3, 0.7)
        )
        this.tweens.add({
          targets: star,
          alpha: Phaser.Math.FloatBetween(0.1, 0.9),
          duration: Phaser.Math.Between(1000, 3000),
          yoyo: true,
          repeat: -1
        })
      }

      // Crear el jugador (cesta)
      this.player = this.add.graphics()
      this.player.fillStyle(0x8b5cf6, 1)
      this.player.fillRoundedRect(-40, -10, 80, 20, 10)
      this.player.fillStyle(0xa78bfa, 1)
      this.player.fillRoundedRect(-35, -8, 70, 16, 8)
      this.player.x = W / 2
      this.player.y = H - 40

      // Grupos para objetos que caen
      this.stars = []
      this.bombs = []

      // Input
      this.cursors = this.input.keyboard.createCursorKeys()
      this.wasd = this.input.keyboard.addKeys({ left: 'A', right: 'D' })

      // Timer para generar objetos
      this.time.addEvent({
        delay: 600, // Objetos aparecen más rápido
        callback: this.spawnFallingObject,
        callbackScope: this,
        loop: true
      })

      // Variables de estado
      this.playerSpeed = 350
      this.gameActive = true
    }

    spawnFallingObject() {
      if (!this.gameActive) return

      const W = this.scale.width
      const x = Phaser.Math.Between(50, W - 50)
      
      // 70% probabilidad de estrella, 30% de bomba
      const isStar = Math.random() < 0.7

      if (isStar) {
        // Crear estrella
        const star = this.add.graphics()
        star.fillStyle(0xfbbf24, 1)
        this.drawStar(star, 0, 0, 5, 12, 6)
        star.x = x
        star.y = -20
        star.speed = Phaser.Math.Between(150, 250)
        star.isStar = true

        // Efecto de brillo
        this.tweens.add({
          targets: star,
          scaleX: 1.2,
          scaleY: 1.2,
          duration: 500,
          yoyo: true,
          repeat: -1
        })

        this.stars.push(star)
      } else {
        // Crear bomba
        const bomb = this.add.graphics()
        bomb.fillStyle(0xef4444, 1)
        bomb.fillCircle(0, 0, 10)
        bomb.fillStyle(0x991b1b, 1)
        bomb.fillCircle(0, 0, 7)
        // Mecha
        bomb.lineStyle(2, 0x404040, 1)
        bomb.lineBetween(0, -10, 0, -18)
        bomb.fillStyle(0xfbbf24, 1)
        bomb.fillCircle(0, -18, 3)
        
        bomb.x = x
        bomb.y = -20
        bomb.speed = Phaser.Math.Between(180, 280)
        bomb.isStar = false

        // Animación de parpadeo de la mecha
        this.tweens.add({
          targets: bomb,
          alpha: 0.6,
          duration: 200,
          yoyo: true,
          repeat: -1
        })

        this.bombs.push(bomb)
      }
    }

    drawStar(graphics, cx, cy, spikes, outerRadius, innerRadius) {
      let rot = Math.PI / 2 * 3
      let x = cx
      let y = cy
      const step = Math.PI / spikes

      graphics.beginPath()
      graphics.moveTo(cx, cy - outerRadius)
      
      for (let i = 0; i < spikes; i++) {
        x = cx + Math.cos(rot) * outerRadius
        y = cy + Math.sin(rot) * outerRadius
        graphics.lineTo(x, y)
        rot += step

        x = cx + Math.cos(rot) * innerRadius
        y = cy + Math.sin(rot) * innerRadius
        graphics.lineTo(x, y)
        rot += step
      }
      
      graphics.lineTo(cx, cy - outerRadius)
      graphics.closePath()
      graphics.fillPath()
    }

    checkCollision(obj, player) {
      const distance = Phaser.Math.Distance.Between(obj.x, obj.y, player.x, player.y)
      return distance < 50
    }

    update(time, delta) {
      if (!this.gameActive) return

      const W = this.scale.width
      const H = this.scale.height

      // Movimiento del jugador
      const left = this.cursors.left.isDown || this.wasd.left.isDown
      const right = this.cursors.right.isDown || this.wasd.right.isDown

      if (left) {
        this.player.x = Math.max(50, this.player.x - this.playerSpeed * delta / 1000)
      } else if (right) {
        this.player.x = Math.min(W - 50, this.player.x + this.playerSpeed * delta / 1000)
      }

      // Actualizar estrellas
      for (let i = this.stars.length - 1; i >= 0; i--) {
        const star = this.stars[i]
        star.y += star.speed * delta / 1000

        // Verificar colisión
        if (this.checkCollision(star, this.player)) {
          score.value++
          this.createParticles(star.x, star.y, 0xfbbf24)
          star.destroy()
          this.stars.splice(i, 1)

          // Verificar victoria
          if (score.value >= targetScore) {
            this.gameActive = false
            showVictory.value = true
          }
          continue
        }

        // Eliminar si sale de la pantalla
        if (star.y > H + 20) {
          star.destroy()
          this.stars.splice(i, 1)
        }
      }

      // Actualizar bombas
      for (let i = this.bombs.length - 1; i >= 0; i--) {
        const bomb = this.bombs[i]
        bomb.y += bomb.speed * delta / 1000

        // Verificar colisión
        if (this.checkCollision(bomb, this.player)) {
          lives.value--
          this.createExplosion(bomb.x, bomb.y)
          bomb.destroy()
          this.bombs.splice(i, 1)

          // Verificar game over
          if (lives.value <= 0) {
            this.gameActive = false
            gameOver.value = true
          }
          continue
        }

        // Eliminar si sale de la pantalla
        if (bomb.y > H + 20) {
          bomb.destroy()
          this.bombs.splice(i, 1)
        }
      }
    }

    createParticles(x, y, color) {
      for (let i = 0; i < 8; i++) {
        const particle = this.add.circle(x, y, 3, color)
        const angle = (Math.PI * 2 * i) / 8
        const speed = Phaser.Math.Between(100, 200)
        
        this.tweens.add({
          targets: particle,
          x: x + Math.cos(angle) * 50,
          y: y + Math.sin(angle) * 50,
          alpha: 0,
          duration: 500,
          onComplete: () => particle.destroy()
        })
      }
    }

    createExplosion(x, y) {
      for (let i = 0; i < 12; i++) {
        const particle = this.add.circle(x, y, Phaser.Math.Between(4, 8), 0xff6b6b)
        const angle = (Math.PI * 2 * i) / 12
        const speed = Phaser.Math.Between(150, 300)
        
        this.tweens.add({
          targets: particle,
          x: x + Math.cos(angle) * 80,
          y: y + Math.sin(angle) * 80,
          alpha: 0,
          scaleX: 0,
          scaleY: 0,
          duration: 600,
          onComplete: () => particle.destroy()
        })
      }

      // Flash de la pantalla
      const flash = this.add.rectangle(
        this.scale.width / 2,
        this.scale.height / 2,
        this.scale.width,
        this.scale.height,
        0xff0000,
        0.3
      )
      this.tweens.add({
        targets: flash,
        alpha: 0,
        duration: 200,
        onComplete: () => flash.destroy()
      })
    }
  }

  // Configuración de Phaser
  const config = {
    type: P.AUTO,
    width: 800,
    height: 500,
    backgroundColor: '#0f172a',
    parent: gameCanvas.value,
    scene: GameScene,
    physics: {
      default: 'arcade',
      arcade: { debug: false }
    }
  }

  game = new P.Game(config)
})

onUnmounted(() => {
  if (game) {
    game.destroy(true)
    game = null
  }
})
</script>

<style scoped lang="scss">
.game-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(to bottom, #0f172a, #1e1b4b);
  font-family: 'Inter', sans-serif;
  padding: 0;
}

.hud {
  width: 800px;
  max-width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 24px;
  background: rgba(15, 23, 42, 0.9);
  backdrop-filter: blur(10px);
  border-bottom: 2px solid rgba(139, 92, 246, 0.3);

  .back-btn {
    display: flex;
    align-items: center;
    gap: 6px;
    color: #a78bfa;
    text-decoration: none;
    font-size: 14px;
    font-weight: 600;
    padding: 8px 16px;
    border: 1px solid rgba(167, 139, 250, 0.3);
    border-radius: 10px;
    transition: all 0.3s;

    &:hover {
      background: rgba(167, 139, 250, 0.15);
      border-color: rgba(167, 139, 250, 0.6);
      transform: translateX(-2px);
    }
  }

  .score-display {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 32px;
    font-weight: 700;
    color: #fbbf24;
    text-shadow: 0 0 20px rgba(251, 191, 36, 0.5);
  }

  .lives-display {
    display: flex;
    gap: 6px;
    color: #ef4444;
    
    .life-lost {
      color: #4b5563;
      opacity: 0.3;
    }
  }
}

.game-area {
  width: 800px;
  max-width: 100%;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);

  canvas {
    display: block;
    width: 100% !important;
    height: auto !important;
  }
}

.instructions {
  width: 800px;
  max-width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  padding: 16px 24px;
  background: rgba(15, 23, 42, 0.9);
  backdrop-filter: blur(10px);
  border-top: 2px solid rgba(139, 92, 246, 0.3);
  font-size: 13px;
  font-weight: 500;
  color: #94a3b8;

  span {
    display: flex;
    align-items: center;
    gap: 4px;
  }
}

.victory-overlay,
.gameover-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(8px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.victory-card,
.gameover-card {
  background: linear-gradient(135deg, #1e1b4b 0%, #312e81 100%);
  border: 2px solid #8b5cf6;
  border-radius: 20px;
  padding: 40px 60px;
  text-align: center;
  box-shadow: 0 20px 60px rgba(139, 92, 246, 0.4);
  animation: slideUp 0.4s ease;

  h2 {
    font-size: 42px;
    font-weight: 800;
    color: #fbbf24;
    margin: 0 0 16px 0;
    text-shadow: 0 0 30px rgba(251, 191, 36, 0.6);
  }

  p {
    font-size: 18px;
    color: #cbd5e1;
    margin: 8px 0;
  }

  .target-info {
    color: #94a3b8;
    font-size: 16px;
  }
}

@keyframes slideUp {
  from {
    transform: translateY(30px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

.trophy-icon {
  color: #fbbf24;
  margin-bottom: 16px;
  animation: bounce 1s ease infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

.cv-button,
.restart-button {
  margin-top: 20px;
  padding: 14px 32px;
  font-size: 16px;
  font-weight: 700;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s;
  display: inline-flex;
  align-items: center;
  gap: 8px;
}

.cv-button {
  background: linear-gradient(135deg, #8b5cf6 0%, #a78bfa 100%);
  color: white;
  margin-right: 12px;
  box-shadow: 0 4px 20px rgba(139, 92, 246, 0.4);

  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 30px rgba(139, 92, 246, 0.6);
  }

  &:active {
    transform: translateY(0);
  }
}

.restart-button {
  background: rgba(255, 255, 255, 0.1);
  color: #e2e8f0;
  border: 2px solid rgba(255, 255, 255, 0.2);

  &:hover {
    background: rgba(255, 255, 255, 0.15);
    border-color: rgba(255, 255, 255, 0.3);
    transform: translateY(-2px);
  }

  &:active {
    transform: translateY(0);
  }
}
</style>