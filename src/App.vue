<script setup lang="ts">
import {Citrus, MessageSquare, LucideIcon, Gamepad} from "@lucide/vue";
import {nextTick, onMounted, ref} from "vue";
import type { Component } from "vue";
import LemonGame from "@/components/LemonGame.vue";
import LemonReviews from "@/components/LemonReviews.vue";
import LemonOverview from "@/components/LemonOverview.vue";

type NavigationItem = {
  title: string;
  component: Component;
  icon: LucideIcon;
};

const items: NavigationItem[] = [
  { title: "Overview", component: LemonOverview, icon: Citrus },
  { title: "Reviews", component: LemonReviews, icon: MessageSquare },
  { title: "Lemon Game", component: LemonGame, icon: Gamepad },
];

const activeIndex = ref<number>(0);
const buttonRefs = ref<HTMLButtonElement[]>([]);

const boxStyle = ref({
  width: "0px",
  transform: "translateX(0px)",
});

const updateBoxPosition = () => {
  const currentButton = buttonRefs.value[activeIndex.value];

  if (currentButton) {
    boxStyle.value = {
      width: `${currentButton.offsetWidth}px`,
      transform: `translateX(${currentButton.offsetLeft}px)`
    };
  }
};

const selectItem = (index: number) => {
  activeIndex.value = index;
  updateBoxPosition();
};

onMounted(() => {
  nextTick(() => {
    updateBoxPosition();
  });
});
</script>

<template>
  <div class="container">
    <div class="header-container">
      <img class="logo" src="/lemon.png" alt="lemon">
      <h1 class="header">Lemon Soda</h1>
    </div>

    <main class="content">
      <component :is="items[activeIndex].component" />
    </main>

    <div class="navigation">
      <button
          v-for="(item, index) in items"
          :key="item.title"
          ref="buttonRefs"
          class="navigation-button"
          :class="{ active: activeIndex === index}"
          @click="selectItem(index)"
      >
        <component :is="item.icon" class="icon"/>
        {{ item.title }}
      </button>

      <div class="navigation-box" :style="boxStyle"></div>
    </div>
  </div>

</template>

<style scoped>
.container {
  padding: 1.4rem 1.6rem;
  display: flex;
  flex-direction: column;
  height: 100dvh;
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
}

.header-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  font-size: 1rem;
  flex-shrink: 0;
}

.content {
  flex: 1;
  min-height: 0;
  overflow-y: auto;
  padding: 1.5rem;
  width: 100%;
  box-sizing: border-box;
}

.logo {
  height: 4rem;
}

.navigation {
  position: relative;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 0 0.4rem;
  border-top: 0.2rem solid var(--vt-c-divider-dark-1);
  flex-shrink: 0;
  margin-top: auto;
  width: 100%;
  box-sizing: border-box;
  background-color: var(--color-background, #1a1a1a);
}

.navigation-button {
  display: flex;
  align-items: center;
  background: none;
  border: none;
  color: var(--color-text);
  font-weight: bold;
  cursor: pointer;
  z-index: 2;
  transition: color 0.3s ease;
}

.navigation-button {
  padding: 0.5rem 1.2rem;
  font-size: 0.8rem;
  gap: 0.3rem;
}

.navigation-button:hover {
  color: var(--vt-c-white-soft);
}

.navigation-button.active {
  color: var(--color-primary);
}

.icon {
  width: 1.2rem;
  height: 1.2rem;
  flex-shrink: 0;
}

.navigation-box {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  outline: 0.15rem solid var(--color-primary);
  border-radius: 0.3rem;
  pointer-events: none;
  z-index: 1;
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1), width 0.3s ease;
}
</style>