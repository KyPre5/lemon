<script setup lang="ts">
import { ref } from "vue";

type Achievement = {
  title: string;
  description: string;
};

const achievements: Achievement[] = [
  {
    title: 'Almost a Marathon',
    description: 'Walked more than 25km in one day',
  },
  {
    title: 'Lemon Expert',
    description: 'Drank more than 10 Lemon Sodas',
  },
  {
    title: 'No Oranges Allowed',
    description: 'Drank more than 20 Lemon Sodas',
  },
  {
    title: 'A echta Linza',
    description: 'Explored 100% of Linz',
  },
  {
    title: 'One More Lemon Soda?',
    description: 'Spent more than 10 hours together in one day',
  }
];

const activeIndex = ref<number | null>(0);

const toggleAchievement = (index: number) => {
  activeIndex.value = activeIndex.value === index ? null : index;
};
</script>

<template>
  <div class="container">
    <h2 class="header">Achievements</h2>

    <div class="achievements">
      <div
          v-for="(item, index) in achievements"
          :key="item.title"
          class="achievement-card"
      >
        <div class="achievement" @click="toggleAchievement(index)">
          <img class="checked" src="/trophy.png" alt="trophy">
          <h4>{{ item.title }}</h4>
        </div>

        <p v-if="activeIndex === index" class="description">
          {{ item.description }}
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container{
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 0.5rem;
}

.achievements {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.achievement{
  display: flex;
  flex-direction: row;
  align-items: center;
  background: var(--color-background-soft);
  border-radius: 0 1rem;
}


.checked {
  width: 3rem;
  height: 3rem;
}

.achievement-card {
  outline: #f7d070 0.12rem solid;
  border-radius: 0 1rem;
}

.description {
  padding: 0.4rem;
}
</style>