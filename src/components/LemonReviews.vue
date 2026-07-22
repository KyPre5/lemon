<script setup lang="ts">

type Review = {
  id: number;
  location: string;
  comment: string;
  score: number;
  side: "left" | "right";
};

const reviews: Review[] = [
  {
    id: 1,
    location: "Le Burger",
    comment: "Perfekt um ein wenig länger zu sitzen und ein Sozi mit Eiswürfeln zu genießen",
    score: 5,
    side: "left",
  },
  {
    id: 2,
    location: "l'Osteria",
    comment: "Eine gute Alternative, falls sonst nichts mehr offen hat. Leider keine Eiswürfel oder Zitronenscheiben",
    score: 3,
    side: "right",
  },
  {
    id: 3,
    location: "Bäckerei Winkler",
    comment: "Ruhiger und entspannter Ort um ein Sozi zu trinken oder auch einen Kaffee",
    score: 4,
    side: "left",
  },
  {
    id: 4,
    location: "Café Central",
    comment: "Große Gläser und sogar Zitronenscheiben vorhanden",
    score: 4,
    side: "left",
  },
  {
    id: 5,
    location: "Turm 20",
    comment: "Nichtmal ein Zitronensaft. Einfach nur 3 Zitronenscheiben in einem Glas und 1 Mineralwasser",
    score: 2,
    side: "right",
  }
];

const maxScore: number = 5;

const getStarIcon = (score: number, index: number) => {
  const fileName = index <= score ? 'yellow-star.png' : 'grey-star.png';
  return `${import.meta.env.BASE_URL}${fileName}`;
};
</script>

<template>
  <div class="chat-container">
    <h1>Reviews</h1>

    <div
        v-for="review in reviews"
        :key="review.id"
        :class="['review-bubble', review.side]"
    >
      <div class="bubble-header">
        <span class="location">{{ review.location }}</span>
      </div>

      <p class="comment">{{ review.comment }}</p>

      <div class="score">
        <img
            v-for="i in maxScore"
            :key="i"
            class="player-head"
            :src="getStarIcon(review.score, i)"
            alt="player-head-score"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  max-width: 500px;
  margin: 0 auto;
  font-family: monospace, sans-serif;
}

.review-bubble {
  position: relative;
  max-width: 75%;
  padding: 0.8rem 1rem;
  background-color: #2b2b2b;
  border: 3px solid #444;
  border-radius: 8px;
  color: #fff;
}

.review-bubble.left {
  align-self: flex-start;
  border-bottom-left-radius: 0;
}

.review-bubble.right {
  align-self: flex-end;
  background-color: rgb(247 208 112 / 0.2);
  border-color: #f7d070;
  border-bottom-right-radius: 0;
}

.location {
  font-size: 1.1rem;
  font-weight: bold;
  color: #f7d070;
}

.comment {
  margin: 0.4rem 0 0.8rem 0;
  font-size: 0.95rem;
  line-height: 1.3;
  word-break: break-word;
}

.score {
  display: flex;
  gap: 0.3rem;
}

.player-head {
  width: 2rem;
  height: 2rem;
}
</style>