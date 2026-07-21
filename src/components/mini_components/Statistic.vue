<script setup lang="ts">

type Item = {
  key: string,
  value: string
};

const items: Item[] = [
  {
    key: "Favorite Place",
    value: "The AU"
  },
  {
    key: "Longest Walk",
    value: "17.07.2026 (27.727km)"
  },
  {
    key: "Steps Together",
    value: "157.396 (last 30 days)"
  },
  {
    key: "First Lemon Soda",
    value: "Lentia (~4.5 years ago)"
  },
  {
    key: "Coffee Gap",
    value: "Still hugggeee"
  }
];

</script>

<template>
  <div class="statistic-container">
    <h2 class="header">Statistic</h2>

    <div class="statistic">
      <div
          v-for="item in items"
          class="stat"
      >
        <h4>{{ item.key }}</h4>
        <p>{{ item.value }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>

.stat {
  display: flex;
  justify-content: space-between;
}

</style>