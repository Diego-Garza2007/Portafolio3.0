<template>
  <div class="game-wrap">
    <!-- HUD -->
    <div class="hud">
      <NuxtLink to="/" class="hud__back">
        <Icon name="material-symbols:arrow-back-rounded" size="18" />
        Inicio
      </NuxtLink>
      <div class="hud__center">
        <span class="hud__title">HOLLOW RUN</span>
      </div>
      <div class="hud__stats">
        <span class="hud__gems">
          <Icon name="material-symbols:diamond-rounded" size="16" />
          {{ gems }}
        </span>
        <div class="hud__health">
          <span
            v-for="i in maxHealth"
            :key="i"
            class="hud__heart"
            :class="{ 'hud__heart--empty': i > health }"
          >
            <Icon name="material-symbols:favorite-rounded" size="16" />
          </span>
        </div>
      </div>
    </div>

    <!-- Game canvas container -->
    <div ref="gameContainer" class="game-canvas"></div>

    <!-- Controls hint -->
    <div class="controls">
      <span><Icon name="material-symbols:keyboard-arrow-left-rounded" size="14" /> <Icon name="material-symbols:keyboard-arrow-right-rounded" size="14" /> Mover</span>
      <span><Icon name="material-symbols:keyboard-arrow-up-rounded" size="14" /> / Z Saltar</span>
      <span>X Atacar</span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const gameContainer = ref(null)
const gems   = ref(0)
const health = ref(3)
const maxHealth = ref(3)

let game = null

onMounted(async () => {
  // Load Phaser dynamically
  const Phaser = await import('https://cdn.jsdelivr.net/npm/phaser@3.80.1/dist/phaser.esm.js')
  const P = Phaser.default ?? Phaser

  // ── Palette ──────────────────────────────────────────────
  const C = {
    bg:       0x0b0b2e,
    bgMid:    0x10102a,
    platform: 0x1e1b4b,
    platEdge: 0x4f46e5,
    gem:      0x818cf8,
    gemGlow:  0xa78bfa,
    enemy:    0xdc2626,
    enemyEye: 0xfca5a5,
    player:   0xe2e8f0,
    playerSoul:0x818cf8,
    spike:    0x7c3aed,
    text:     0xc4b5fd,
    wall:     0x1a1740,
    wallEdge: 0x312e81,
    particle: 0xa78bfa,
  }

  // ── Helpers ───────────────────────────────────────────────
  function drawPlayer(gfx) {
    gfx.clear()
    // Cloak
    gfx.fillStyle(0x08080f)
    gfx.fillEllipse(0, 14, 28, 30)
    // Body
    gfx.fillStyle(0x0f0e24)
    gfx.fillRect(-10, -2, 20, 18)
    // Head
    gfx.fillStyle(0x0f0e24)
    gfx.fillRoundedRect(-11, -18, 22, 18, 4)
    // Horns
    gfx.fillStyle(0x0a0918)
    gfx.fillTriangle(-8, -17, -14, -28, -4, -17)
    gfx.fillTriangle(8, -17, 14, -28, 4, -17)
    // Eyes
    gfx.fillStyle(0xf5f0ff)
    gfx.fillCircle(-5, -11, 4)
    gfx.fillCircle(5, -11, 4)
    // Soul
    gfx.fillStyle(0x818cf8, 0.9)
    gfx.fillCircle(0, 6, 4)
  }

  function drawEnemy(gfx) {
    gfx.clear()
    gfx.fillStyle(C.enemy)
    gfx.fillRoundedRect(-12, -10, 24, 20, 4)
    gfx.fillStyle(C.enemyEye)
    gfx.fillCircle(-4, -3, 4)
    gfx.fillCircle(4, -3, 4)
    gfx.fillStyle(0x991b1b)
    gfx.fillCircle(-4, -3, 2)
    gfx.fillCircle(4, -3, 2)
    // Spikes on top
    gfx.fillStyle(0xb91c1c)
    gfx.fillTriangle(-8, -10, -5, -18, -2, -10)
    gfx.fillTriangle(2, -10, 5, -18, 8, -10)
  }

  // ── Scene ─────────────────────────────────────────────────
  class GameScene extends P.Scene {
    constructor() { super({ key: 'GameScene' }) }

    create() {
      const W = this.scale.width
      const H = this.scale.height

      // Background gradient (layered rects)
      this.add.rectangle(W/2, H/2, W, H, C.bg)
      this.add.rectangle(W/2, H*0.75, W, H*0.5, C.bgMid, 0.4)

      // ── Static world ──────────────────────────────────────
      this.platforms = this.physics.add.staticGroup()

      const platData = [
        // [x, y, w, h]
        [W/2,  H-20,  W,    20],   // floor
        [0,    H/2,   20,   H],    // left wall
        [W,    H/2,   20,   H],    // right wall
        [150,  H-120, 160,  16],
        [420,  H-180, 200,  16],
        [680,  H-140, 160,  16],
        [260,  H-280, 180,  16],
        [560,  H-320, 160,  16],
        [100,  H-400, 200,  16],
        [450,  H-440, 180,  16],
        [700,  H-380, 140,  16],
        [W/2,  H-520, 240,  16],   // top center
      ]

      platData.forEach(([x, y, w, h]) => {
        const rect = this.add.graphics()
        rect.fillStyle(C.platform)
        rect.fillRect(-w/2, -h/2, w, h)
        rect.fillStyle(C.platEdge, 0.6)
        rect.fillRect(-w/2, -h/2, w, 3)  // top edge glow
        rect.x = x; rect.y = y
        const body = this.physics.add.existing(rect, true)
        body.body.setSize(w, h)
        this.platforms.add(body)
      })

      // ── Spikes ────────────────────────────────────────────
      this.spikes = this.physics.add.staticGroup()
      const spikePositions = [
        [300, H-35], [320, H-35], [340, H-35],
        [600, H-35], [620, H-35],
      ]
      spikePositions.forEach(([sx, sy]) => {
        const sg = this.add.graphics()
        sg.fillStyle(C.spike)
        sg.fillTriangle(0, 10, 8, -4, 16, 10)
        sg.x = sx - 8; sg.y = sy - 10
        const sb = this.physics.add.existing(sg, true)
        sb.body.setSize(16, 14).setOffset(0, 0)
        this.spikes.add(sb)
      })

      // ── Gems ──────────────────────────────────────────────
      this.gemGroup = this.physics.add.staticGroup()
      const gemPositions = [
        [150, H-148], [180, H-148], [210, H-148],
        [420, H-208], [460, H-208], [500, H-208],
        [260, H-308], [290, H-308],
        [560, H-348], [580, H-348], [600, H-348],
        [100, H-428], [130, H-428],
        [W/2, H-548], [W/2-30, H-548], [W/2+30, H-548],
      ]
      this.gemGraphics = []
      gemPositions.forEach(([gx, gy]) => {
        const gg = this.add.graphics()
        gg.fillStyle(C.gem, 0.95)
        gg.fillRect(-5, -8, 10, 16)
        gg.fillStyle(C.gemGlow, 0.5)
        gg.fillCircle(0, 0, 7)
        gg.x = gx; gg.y = gy
        const gb = this.physics.add.existing(gg, true)
        gb.body.setSize(10, 16)
        this.gemGroup.add(gb)
        this.gemGraphics.push(gg)
      })

      // ── Enemies ───────────────────────────────────────────
      this.enemies = this.physics.add.group()
      const enemyData = [
        [150, H-140], [500, H-200], [680, H-160],
        [260, H-300], [100, H-420], [W/2, H-540],
      ]
      this.enemyList = []
      enemyData.forEach(([ex, ey]) => {
        const eg = this.add.graphics()
        drawEnemy(eg)
        const eb = this.physics.add.existing(eg)
        eb.body.setSize(24, 20).setOffset(-12, -10)
        eb.body.setCollideWorldBounds(true)
        eb.body.setBounceX(1)
        eb.body.setVelocityX(Phaser.Math.Between(-60, 60) || 50)
        eg.x = ex; eg.y = ey
        this.enemies.add(eb)
        this.enemyList.push({ gfx: eg, dir: 1 })
      })

      // ── Player ────────────────────────────────────────────
      this.playerGfx = this.add.graphics()
      drawPlayer(this.playerGfx)
      this.playerBody = this.physics.add.existing(this.playerGfx)
      this.playerBody.body
        .setSize(18, 30)
        .setOffset(-9, -18)
        .setCollideWorldBounds(true)
        .setGravityY(100)
      this.playerBody.x = W / 2
      this.playerBody.y = H - 80

      // ── Sword hitbox ──────────────────────────────────────
      this.swordActive = false
      this.swordTimer  = 0
      this.swordGfx = this.add.graphics()
      this.swordBody = this.physics.add.existing(this.swordGfx)
      this.swordBody.body.setSize(0, 0)
      this.swordBody.body.allowGravity = false

      // ── Colliders ─────────────────────────────────────────
      this.physics.add.collider(this.playerBody, this.platforms)
      this.physics.add.collider(this.enemies,    this.platforms)
      this.physics.add.collider(this.enemies,    this.enemies)

      this.physics.add.overlap(this.playerBody, this.gemGroup, (p, gem) => {
        gem.destroy()
        gems.value++
        // Sparkle particles via simple graphics
        this.spawnParticles(gem.x, gem.y)
      })

      this.physics.add.overlap(this.playerBody, this.spikes, () => {
        this.hurtPlayer()
      })

      this.physics.add.overlap(this.playerBody, this.enemies, (p, enemy) => {
        if (!this.invincible) this.hurtPlayer()
      })

      this.physics.add.overlap(this.swordBody, this.enemies, (sword, enemy) => {
        if (this.swordActive) {
          this.spawnParticles(enemy.x, enemy.y)
          enemy.destroy()
        }
      })

      // ── Input ─────────────────────────────────────────────
      this.cursors = this.input.keyboard.createCursorKeys()
      this.wasd    = this.input.keyboard.addKeys({ up: 'W', left: 'A', right: 'D', attack: 'X', jump2: 'Z' })

      // ── State ─────────────────────────────────────────────
      this.invincible    = false
      this.invincibleTimer = 0
      this.facingRight   = true
      this.isAttacking   = false
      this.attackTimer   = 0
      this.canDoubleJump = false
      this.hasJumped     = false
      this.particles     = []
      this.gemBob        = 0
    }

    spawnParticles(x, y) {
      for (let i = 0; i < 6; i++) {
        const p = this.add.graphics()
        p.fillStyle(C.particle, 0.9)
        p.fillCircle(0, 0, Phaser.Math.Between(2, 4))
        p.x = x; p.y = y
        p.vx = Phaser.Math.Between(-80, 80)
        p.vy = Phaser.Math.Between(-100, -30)
        p.life = 1.0
        this.particles.push(p)
      }
    }

    hurtPlayer() {
      if (this.invincible) return
      health.value = Math.max(0, health.value - 1)
      this.invincible = true
      this.invincibleTimer = 1200
      // Knockback
      this.playerBody.body.setVelocityY(-200)
      if (health.value <= 0) this.scene.restart()
    }

    update(time, delta) {
      const dt     = delta / 1000
      const body   = this.playerBody.body
      const onFloor = body.blocked.down
      const left   = this.cursors.left.isDown  || this.wasd.left.isDown
      const right  = this.cursors.right.isDown || this.wasd.right.isDown
      const jump   = P.Input.Keyboard.JustDown(this.cursors.up) ||
                     P.Input.Keyboard.JustDown(this.wasd.up)    ||
                     P.Input.Keyboard.JustDown(this.wasd.jump2)
      const attack = P.Input.Keyboard.JustDown(this.wasd.attack)

      // ── Movement ──────────────────────────────────────────
      if (left)       { body.setVelocityX(-200); this.facingRight = false }
      else if (right) { body.setVelocityX(200);  this.facingRight = true  }
      else            { body.setVelocityX(body.velocity.x * 0.8) }

      // ── Jump / double jump ────────────────────────────────
      if (jump) {
        if (onFloor) {
          body.setVelocityY(-480)
          this.canDoubleJump = true
        } else if (this.canDoubleJump) {
          body.setVelocityY(-420)
          this.canDoubleJump = false
        }
      }

      // ── Attack ────────────────────────────────────────────
      if (attack && !this.isAttacking) {
        this.isAttacking  = true
        this.swordActive  = true
        this.attackTimer  = 0.25
        const dir = this.facingRight ? 1 : -1
        const sx  = this.playerGfx.x + dir * 28
        const sy  = this.playerGfx.y
        this.swordBody.x = sx
        this.swordBody.y = sy
        this.swordBody.body.setSize(28, 14).setOffset(-14, -7)
      }

      if (this.isAttacking) {
        this.attackTimer -= dt
        // Draw sword arc
        this.swordGfx.clear()
        this.swordGfx.lineStyle(3, 0xd4c8f0, 0.9)
        const dir = this.facingRight ? 1 : -1
        this.swordGfx.strokeRect(-14, -7, 28, 14)
        // Visible nail
        this.swordGfx.lineStyle(3.5, 0xd4c8f0, 1)
        this.swordGfx.lineBetween(dir * -6, 0, dir * 22, 0)
        this.swordBody.x = this.playerGfx.x + dir * 24
        this.swordBody.y = this.playerGfx.y

        if (this.attackTimer <= 0) {
          this.isAttacking = false
          this.swordActive = false
          this.swordGfx.clear()
          this.swordBody.body.setSize(0, 0)
        }
      }

      // ── Invincibility flash ───────────────────────────────
      if (this.invincible) {
        this.invincibleTimer -= delta
        this.playerGfx.setAlpha(Math.sin(time * 0.02) > 0 ? 1 : 0.3)
        if (this.invincibleTimer <= 0) {
          this.invincible = false
          this.playerGfx.setAlpha(1)
        }
      }

      // ── Sync player gfx position ──────────────────────────
      this.playerGfx.setScale(this.facingRight ? 1 : -1, 1)

      // ── Gem bobbing ───────────────────────────────────────
      this.gemBob += dt * 2
      this.gemGroup.getChildren().forEach((gem, i) => {
        gem.y += Math.sin(this.gemBob + i) * 0.4
      })

      // ── Particles ─────────────────────────────────────────
      for (let i = this.particles.length - 1; i >= 0; i--) {
        const p = this.particles[i]
        p.life -= dt * 1.5
        p.x += p.vx * dt
        p.y += p.vy * dt
        p.vy += 200 * dt
        p.setAlpha(p.life)
        p.setScale(p.life)
        if (p.life <= 0) {
          p.destroy()
          this.particles.splice(i, 1)
        }
      }
    }
  }

  // ── Phaser config ─────────────────────────────────────────
  const config = {
    type: P.AUTO,
    width:  800,
    height: 550,
    backgroundColor: '#0b0b2e',
    parent: gameContainer.value,
    physics: {
      default: 'arcade',
      arcade: { gravity: { y: 600 }, debug: false }
    },
    scene: GameScene,
  }

  game = new P.Game(config)
  // Reset reactive state on (re)start
  const origCreate = GameScene.prototype.create
  GameScene.prototype.create = function () {
    health.value = 3
    gems.value   = 0
    origCreate.call(this)
  }
})

onUnmounted(() => {
  if (game) { game.destroy(true); game = null }
})
</script>

<style scoped lang="scss">
.game-wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  min-height: 100vh;
  background: #06060f;
  font-family: 'Inter', sans-serif;
  padding: 0;
}

/* ── HUD ── */
.hud {
  width: 800px;
  max-width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 20px;
  background: rgba(11, 11, 46, 0.95);
  border-bottom: 1px solid rgba(79, 70, 229, 0.35);

  &__back {
    display: flex;
    align-items: center;
    gap: 6px;
    color: #a78bfa;
    text-decoration: none;
    font-size: 13px;
    font-weight: 600;
    letter-spacing: 0.03em;
    padding: 6px 12px;
    border: 1px solid rgba(167, 139, 250, 0.25);
    border-radius: 8px;
    transition: all 0.2s;

    &:hover {
      background: rgba(167, 139, 250, 0.1);
      border-color: rgba(167, 139, 250, 0.5);
      color: #ddd6fe;
    }
  }

  &__center {
    flex: 1;
    text-align: center;
  }

  &__title {
    font-size: 14px;
    font-weight: 700;
    letter-spacing: 0.22em;
    color: #c4b5fd;
  }

  &__stats {
    display: flex;
    align-items: center;
    gap: 14px;
  }

  &__gems {
    display: flex;
    align-items: center;
    gap: 5px;
    font-size: 14px;
    font-weight: 700;
    color: #818cf8;
  }

  &__health {
    display: flex;
    gap: 4px;
  }

  &__heart {
    color: #dc2626;
    transition: color 0.2s;

    &--empty {
      color: #3f3f5a;
    }
  }
}

/* ── Canvas container ── */
.game-canvas {
  width: 800px;
  max-width: 100%;

  canvas {
    display: block;
    width: 100% !important;
    height: auto !important;
  }
}

/* ── Controls hint ── */
.controls {
  width: 800px;
  max-width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 24px;
  padding: 10px 20px;
  background: rgba(11, 11, 46, 0.9);
  border-top: 1px solid rgba(79, 70, 229, 0.2);
  font-size: 12px;
  font-weight: 500;
  color: #6b7280;
  letter-spacing: 0.04em;

  span {
    display: flex;
    align-items: center;
    gap: 2px;
  }
}
</style>