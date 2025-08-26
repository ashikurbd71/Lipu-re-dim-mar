<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const score = ref(0);
const eggs = ref([]);
let burstSound = null;
let eggId = 0;

const addEgg = () => {
  const newEgg = {
    id: eggId++,
    x: Math.random() * (window.innerWidth - 100),
    y: Math.random() * (window.innerHeight - 200) + 100,
    isBursting: false,
  };
  eggs.value.push(newEgg);
};

const burstEgg = (egg) => {
  if (egg.isBursting) return;

  egg.isBursting = true;
  score.value += 10;

  // First: Show burst effect
  setTimeout(() => {
    // Then: Play strong smash sound after burst effect starts
    if (burstSound) {
      try {
        burstSound.currentTime = 0;
        burstSound.volume = 1.0; // Full volume for maximum impact
        const playPromise = burstSound.play();
        if (playPromise !== undefined) {
          playPromise.catch((e) => console.log("Audio play failed:", e));
        }
      } catch (e) {
        console.log("Audio error:", e);
      }
    }

    // Add screen shake effect for impact
    const gameContainer = document.querySelector(".game-container");
    if (gameContainer) {
      gameContainer.style.animation = "screenShake 0.5s ease-in-out";
      setTimeout(() => {
        gameContainer.style.animation = "";
      }, 500);
    }
  }, 200); // Delay sound by 200ms to let burst effect start first

  // Remove egg after 15 seconds
  setTimeout(() => {
    const index = eggs.value.findIndex((e) => e.id === egg.id);
    if (index > -1) {
      eggs.value.splice(index, 1);
    }
  }, 15000);
};

const resetGame = () => {
  score.value = 0;
  eggs.value = [];
  eggId = 0;
};

// Auto-add eggs periodically
let autoAddInterval;

onMounted(() => {
  // Create audio element
  burstSound = new Audio("/sound.mp3");
  burstSound.preload = "auto";

  // Add initial eggs
  for (let i = 0; i < 5; i++) {
    addEgg();
  }
});

onUnmounted(() => {
  if (autoAddInterval) {
    clearInterval(autoAddInterval);
  }
});
</script>

<template>
  <div class="game-container">
    <div class="header">
      <h1>🥚 Egg Burst Game 🥚</h1>
      <div class="score">Score: {{ score }}</div>
    </div>

    <div class="game-area">
      <div
        v-for="egg in eggs"
        :key="egg.id"
        class="egg"
        :class="{ bursting: egg.isBursting }"
        :style="{ left: egg.x + 'px', top: egg.y + 'px' }"
        @click="burstEgg(egg)"
      >
        <div class="egg-shell" v-if="!egg.isBursting">🥚</div>
        <div class="egg-content" v-if="egg.isBursting">
          <img src="/funnyumage.jpg" alt="Funny Image" class="funny-image" />
          <div class="burst-particles">
            <div
              class="particle"
              v-for="i in 8"
              :key="i"
              :style="{ '--angle': i * 45 + 'deg' }"
            ></div>
          </div>
          <div class="burst-ring"></div>
          <div class="burst-flash"></div>
        </div>
      </div>
    </div>

    <div class="controls">
      <button @click="addEgg" class="add-egg-btn">Add Egg</button>
      <button @click="resetGame" class="reset-btn">Reset Game</button>
    </div>

    <div class="footer">
      <p>
        Developed by
        <a href="https://github.com/ashikurbd71" target="_blank" rel="noopener"
          >Ashikur Rahman Ovi</a
        >
      </p>
    </div>
  </div>
</template>

<style scoped>
.game-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  font-family: "Arial", sans-serif;
  overflow: hidden;
  position: relative;
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-bottom: 2px solid rgba(255, 255, 255, 0.2);
}

.header h1 {
  color: white;
  margin: 0;
  font-size: 2.5em;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.score {
  color: white;
  font-size: 1.5em;
  font-weight: bold;
  background: rgba(255, 255, 255, 0.2);
  padding: 10px 20px;
  border-radius: 25px;
  border: 2px solid rgba(255, 255, 255, 0.3);
}

/* Mobile responsive styles */
@media (max-width: 768px) {
  .header {
    padding: 15px;
    flex-direction: column;
    gap: 10px;
  }

  .header h1 {
    font-size: 1.8em;
    text-align: center;
  }

  .score {
    font-size: 1.2em;
    padding: 8px 16px;
  }
}

.game-area {
  position: relative;
  width: 100%;
  height: calc(100vh - 200px);
  overflow: hidden;
}

@media (max-width: 768px) {
  .game-area {
    height: calc(100vh - 180px);
  }
}

.egg {
  position: absolute;
  cursor: pointer;
  transition: transform 0.3s ease;
  z-index: 10;
  touch-action: manipulation;
}

.egg:hover {
  transform: scale(1.1);
}

@media (max-width: 768px) {
  .egg {
    cursor: default;
  }

  .egg:active {
    transform: scale(1.1);
  }
}

.egg-shell {
  font-size: 4em;
  filter: drop-shadow(2px 2px 4px rgba(0, 0, 0, 0.3));
  animation: bounce 2s infinite;
}

@media (max-width: 768px) {
  .egg-shell {
    font-size: 3em;
  }
}

.egg-content {
  position: relative;
  animation: burst 15s ease-out;
}

.burst-particles {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 0;
  height: 0;
}

.particle {
  position: absolute;
  width: 8px;
  height: 8px;
  background: linear-gradient(45deg, #ffeb3b, #ff9800);
  border-radius: 50%;
  animation: particleExplode 1s ease-out forwards;
  transform-origin: center;
}

.particle:nth-child(odd) {
  background: linear-gradient(45deg, #ff5722, #f44336);
}

.particle:nth-child(even) {
  background: linear-gradient(45deg, #4caf50, #8bc34a);
}

.burst-ring {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 0;
  height: 0;
  border: 3px solid #ffeb3b;
  border-radius: 50%;
  animation: ringExpand 0.8s ease-out forwards;
}

.burst-flash {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 0;
  height: 0;
  background: radial-gradient(
    circle,
    rgba(255, 255, 255, 0.8) 0%,
    transparent 70%
  );
  border-radius: 50%;
  animation: flashExpand 0.4s ease-out forwards;
}

.funny-image {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  border: 4px solid white;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  object-fit: cover;
}

@media (max-width: 768px) {
  .funny-image {
    width: 80px;
    height: 80px;
  }
}

.controls {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 20px;
  z-index: 20;
}

@media (max-width: 768px) {
  .controls {
    bottom: 10px;
    gap: 15px;
    flex-direction: column;
    align-items: center;
  }
}

.add-egg-btn,
.reset-btn {
  padding: 12px 24px;
  font-size: 1.1em;
  font-weight: bold;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  min-height: 44px;
  min-width: 44px;
}

@media (max-width: 768px) {
  .add-egg-btn,
  .reset-btn {
    padding: 15px 30px;
    font-size: 1em;
    min-height: 50px;
    min-width: 120px;
  }
}

.footer {
  position: fixed;
  bottom: 5px;
  right: 10px;
  z-index: 30;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  padding: 8px 12px;
  border-radius: 15px;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.footer p {
  margin: 0;
  font-size: 0.8em;
  color: white;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
}

.footer a {
  color: #ffeb3b;
  text-decoration: none;
  font-weight: bold;
  transition: color 0.3s ease;
}

.footer a:hover {
  color: #ffc107;
  text-decoration: underline;
}

@media (max-width: 768px) {
  .footer {
    bottom: 2px;
    right: 5px;
    padding: 6px 10px;
  }

  .footer p {
    font-size: 0.7em;
  }
}

.add-egg-btn {
  background: linear-gradient(45deg, #4caf50, #45a049);
  color: white;
}

.add-egg-btn:hover {
  background: linear-gradient(45deg, #45a049, #4caf50);
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
}

.reset-btn {
  background: linear-gradient(45deg, #f44336, #da190b);
  color: white;
}

.reset-btn:hover {
  background: linear-gradient(45deg, #da190b, #f44336);
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
}

@keyframes burst {
  0% {
    transform: scale(0) rotate(0deg);
    opacity: 0;
  }
  5% {
    transform: scale(1.5) rotate(90deg);
    opacity: 1;
  }
  10% {
    transform: scale(2) rotate(180deg);
    opacity: 1;
  }
  80% {
    transform: scale(1.5) rotate(270deg);
    opacity: 0.8;
  }
  100% {
    transform: scale(0) rotate(360deg);
    opacity: 0;
  }
}

@keyframes particleExplode {
  0% {
    transform: translate(0, 0) scale(0);
    opacity: 1;
  }
  50% {
    transform: translate(
        calc(cos(var(--angle)) * 50px),
        calc(sin(var(--angle)) * 50px)
      )
      scale(1);
    opacity: 1;
  }
  100% {
    transform: translate(
        calc(cos(var(--angle)) * 100px),
        calc(sin(var(--angle)) * 100px)
      )
      scale(0);
    opacity: 0;
  }
}

@keyframes ringExpand {
  0% {
    width: 0;
    height: 0;
    opacity: 1;
  }
  100% {
    width: 200px;
    height: 200px;
    opacity: 0;
  }
}

@keyframes flashExpand {
  0% {
    width: 0;
    height: 0;
    opacity: 1;
  }
  50% {
    width: 150px;
    height: 150px;
    opacity: 0.8;
  }
  100% {
    width: 300px;
    height: 300px;
    opacity: 0;
  }
}

@keyframes screenShake {
  0%,
  100% {
    transform: translateX(0);
  }
  25% {
    transform: translateX(-5px);
  }
  50% {
    transform: translateX(5px);
  }
  75% {
    transform: translateX(-3px);
  }
}

.bursting {
  pointer-events: none;
}
</style>
