<script setup lang="ts">
import { onMounted, onUnmounted, ref } from "vue";

type Ingredient = {
  name: string;
  image: string;
  points: number;
  caught: number;
};

type FallingIngredient = Ingredient & {
  id: number;
  x: number;
  y: number;
  speed: number;
};

const availableIngredients: Ingredient[] = [
  { name: "sugar", image: "sugar-cube.png", points: -10, caught: 0 },
  { name: "orange", image: "orange.png", points: -20, caught: 0 },
  { name: "ice-cube", image: "ice-cube.png", points: 5, caught: 0 },
  { name: "lemon", image: "lemon.png", points: 10, caught: 0 },
];

const fallingItems = ref<FallingIngredient[]>([]);
const totalPoints = ref<number>(0);
const maxHealth: number = 3;
const currentHealth = ref<number>(0);
const isGameStarted = ref<boolean>(false);

let spawnTimeoutId: number | null = null;
let animationFrameId: number | null = null;
let difficultyIntervalId: number | null = null;
let itemIdCounter: number = 0;

const currentSpawnDelay = ref<number>(1500);
const speedMultiplier = ref<number>(1.0);

const stopGameLoop = () => {
  if (spawnTimeoutId) {
    clearTimeout(spawnTimeoutId);
    spawnTimeoutId = null;
  }
  if (difficultyIntervalId) {
    clearInterval(difficultyIntervalId);
    difficultyIntervalId = null;
  }
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId);
    animationFrameId = null;
  }
};

const scheduleNextSpawn = () => {
  if (!isGameStarted.value) return;

  spawnItem();
  spawnTimeoutId = window.setTimeout(scheduleNextSpawn, currentSpawnDelay.value);
};

const spawnItem = () => {
  const randomIndex = Math.floor(Math.random() * availableIngredients.length);
  const randomIngredient = availableIngredients[randomIndex];

  const baseSpeed = 1 + Math.random() * 0.3;

  fallingItems.value.push({
    ...randomIngredient,
    id: itemIdCounter++,
    x: Math.random() * 80,
    y: -50,
    speed: baseSpeed * speedMultiplier.value,
  });
};

const updateGame = () => {
  const containerHeight = 500;
  const remainingItems: FallingIngredient[] = [];

  for (const item of fallingItems.value) {
    const newY = item.y + item.speed;

    if (newY >= containerHeight) {
      if (item.points > 0) {
        currentHealth.value = Math.max(0, currentHealth.value - 1);
      }
    } else {
      remainingItems.push({
        ...item,
        y: newY,
      });
    }
  }

  fallingItems.value = remainingItems;

  if (currentHealth.value > 0) {
    animationFrameId = requestAnimationFrame(updateGame);
  } else {
    stopGameLoop();
    isGameStarted.value = false;
  }
};

const increaseDifficulty = () => {
  speedMultiplier.value *= 1.15;

  currentSpawnDelay.value = Math.max(300, currentSpawnDelay.value - 150);
};

const startGame = () => {
  currentHealth.value = maxHealth;
  totalPoints.value = 0;
  fallingItems.value = [];
  availableIngredients.forEach((item) => (item.caught = 0));

  currentSpawnDelay.value = 1500;
  speedMultiplier.value = 1.0;

  isGameStarted.value = true;
  stopGameLoop();

  scheduleNextSpawn();
  difficultyIntervalId = window.setInterval(increaseDifficulty, 5000);
  animationFrameId = requestAnimationFrame(updateGame);
};

const collectItem = (_item: FallingIngredient) => {
  const found = availableIngredients.find((item) => item.name === _item.name);
  if (found) {
    found.caught++;
    totalPoints.value += found.points;
  }

  fallingItems.value = fallingItems.value.filter((item) => item.id !== _item.id);
};

onMounted(() => {
  currentHealth.value = maxHealth;
});

onUnmounted(() => {
  stopGameLoop();
});
</script>

<template>
  <div class="game-wrapper">
    <div class="header">
      <h1>Points: {{ totalPoints }}</h1>
      <div class="health-bar">
        <img v-for="i in maxHealth" :key="i" :src="(i <= currentHealth ? 'red-heart.png' : 'grey-heart.png')" alt="health-indicator">
      </div>
    </div>

    <div class="game-container">
      <div v-if="!isGameStarted" class="game-overlay">
        <button class="start-btn" @click="startGame">Start Game</button>

        <div class="help">
          <div class="rule">
            <h5>Collect:</h5>
            <span class="entry"><img class="icon" :src="availableIngredients[2].image" :alt="availableIngredients[2].name"> Ice-Cube</span>
            <span class="entry"><img class="icon" :src="availableIngredients[3].image" :alt="availableIngredients[3].name"> Lemon</span>
          </div>
          <div class="rule">
            <h5>Don't Collect:</h5>
            <span class="entry"><img class="icon" :src="availableIngredients[0].image" :alt="availableIngredients[0].name"> Sugar</span>
            <span class="entry"><img class="icon" :src="availableIngredients[1].image" :alt="availableIngredients[1].name"> Orange</span>
          </div>
        </div>
      </div>

      <div
          v-for="item in fallingItems"
          :key="item.id"
          class="falling-item"
          :style="{
            left: `${item.x}%`,
            top: `${item.y}px`,
          }"
          @click="collectItem(item)"
      >
        <img :src="item.image" :alt="item.name" />
      </div>
    </div>

    <div class="stats-container">
      <span class="stat" v-for="item in availableIngredients"><img class="icon" :src="item.image" :alt="item.name">{{ item.caught }}</span>
    </div>
  </div>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.header h1 {
  font-size: 1.6rem;
}

.game-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.game-container {
  position: relative;
  width: 100%;
  max-width: 30rem;
  height: 30rem;
  background-color: var(--color-background-mute);
  border: 2px solid var(--color-primary, #f7d070);
  border-radius: 0.5rem;
  overflow: hidden;
}

.game-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 10;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 3rem;

  background-color: rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(8px);
}

.start-btn {
  margin-top: 5rem;
  padding: 0.8rem 1.6rem;
  font-size: 1.2rem;
  font-weight: bold;
  color: #fff;
  background-color: var(--color-primary, #f7d070);
  border: none;
  border-radius: 0.4rem;
  cursor: pointer;
  transition: transform 0.1s ease-in-out;
}

.start-btn:hover {
  transform: scale(1.05);
}

.falling-item {
  position: absolute;
  width: 4rem;
  height: 4rem;
  pointer-events: auto;
  cursor: pointer;
  user-select: none;
}

.falling-item img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.stats-container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 0 0.2rem;
  margin-top: 0.4rem;
}

.stat {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  padding-right: 0.4rem;
}

.stat .icon {
  width: 3.2rem;
  height: 3.2rem;
}

.health-bar {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.health-bar img {
  width: 3.2rem;
  height: 3.2rem;
}

.help {
  display: flex;
  flex-direction: row;
  padding: 0.4rem;
  gap: 1rem;
}

.rule {
  font-size: 1.2rem;
  padding: 0.2rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.rule img {
  width: 3.2rem;
  height: 3.2rem;
}

.entry {
  font-size: 0.8rem;
  display: flex;
  flex-direction: row;
  align-items: center;
}
</style>