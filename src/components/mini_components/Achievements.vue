<script setup lang="ts">

type Achievement = {
  title: string;
};

const achievements: Achievement[] = [
  {
    title: 'Achievement 1',
  },
  {
    title: 'Achievement 2',
  },
  {
    title: 'Achievement 3',
  },
  {
    title: 'Achievement 4',
  }
];

</script>

<template>
  <div class="container">
    <h2 class="header">Achievements</h2>

    <div class="achievements">
      <div
          v-for="item in achievements"
          class="achievement">
        <img class="checked" src="/trophy.png" alt="trophy">
        <h4>{{ item.title }}</h4>
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

.achievement:hover{
  outline: 0.15rem solid var(--vt-c-text-dark-2);
}

.checked {
  width: 3rem;
  height: 3rem;
}
</style>