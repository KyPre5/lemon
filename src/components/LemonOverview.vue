<script setup>

import Achievements from "@/components/mini_components/Achievements.vue";
import RandomFact from "@/components/mini_components/RandomFact.vue";
import Statistic from "@/components/mini_components/Statistic.vue";
</script>

<template>
  <h1 class="header">Overview</h1>

  <div class="overview-container">
    <Statistic/>

    <Achievements/>

    <RandomFact/>
  </div>

</template>

<style scoped>

.overview-container{
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.header {
  font-size: 2rem;
}

</style>