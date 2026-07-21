<script setup lang="ts">

import {onMounted, ref} from "vue";

type Fact = {
  id: number;
  fact: string;
};

const randomFacts: Fact[] = [
  {
    id: 1,
    fact: "In Österreich gehört Soda Zitron zu den absolut beliebtesten alkoholfreien Erfrischungsgetränken in der Gastronomie."
  },
  {
    id: 2,
    fact: "Ein echtes Soda Zitron besteht aus frisch gepresstem Zitronensaft, der mit spritzigem Soda- bzw. Mineralwasser aufgegossen wird."
  },
  {
    id: 3,
    fact: "Da es meist ohne zugesetzten Zucker zubereitet wird, ist es extrem kalorienarm und die ideale Alternative zu süßen Limonaden."
  },
  {
    id: 4,
    fact: "Die enthaltene Zitrone liefert einen natürlichen Kick an Vitamin C, was das Immunsystem unterstützt."
  },
  {
    id: 5,
    fact: "Es gibt in Lokalen meist zwei Varianten: 'Soda Zitron gepresst' (mit echtem Zitronensaft) oder die günstigere Variante mit Zitronensirup."
  },
  {
    id: 6,
    fact: "Durch die Säure der Zitrone wirkt das Getränk besonders an heißen Sommertagen stark durstlöschend."
  },
  {
    id: 7,
    fact: "Der saure Geschmack regt die Speichelbildung und die Produktion von Magensäure an, weshalb es oft auch vor dem Essen getrunken wird."
  },
  {
    id: 8,
    fact: "Manchmal wird es mit ein paar Blättern frischer Minze oder einer Scheibe Bio-Zitrone als Deko und für noch mehr Aroma serviert."
  },
  {
    id: 9,
    fact: "Es ist ein fester Bestandteil der 'Spritzer-Kultur' und wird oft als alkoholfreie Alternative auf Feiern gewählt."
  },
  {
    id: 10,
    fact: "Die Kombination aus der Säure der Zitrone und der Kohlensäure des Sodawassers sorgt für das typische Prickeln auf der Zunge."
  }
]

const randomFact = ref(null);

const setRandomFact = () => {
  const randomIndex = Math.floor(Math.random() * randomFacts.length);
  randomFact.value = randomFacts[randomIndex];
};

onMounted(() => {
  setRandomFact();
});

</script>

<template>
  <div class="random-fact-container">
    <h2 class="header">Random Fact</h2>

    <h4 v-if="randomFact" class="fact">
      {{ randomFact.fact }}
    </h4>

    <p class="tipp">refresh to get a new one ;)</p>
  </div>
</template>

<style scoped>
.random-fact-container {
}

.tipp {
  font-size: 0.8rem;
}

.fact {
  font-style: italic;
  line-height: 1.4;
  margin-top: 0.5rem;
  color: var(--color-heading);
}
</style>