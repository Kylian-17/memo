<script setup>
import { ref } from "vue"

// --- création des paires ---
const baseCards = [
  { id: 1, value: "", img: "/assets/gr.svg" },
  { id: 2, value: "", img: "/assets/spider.svg" },
  { id: 3, value: "", img: "/assets/plume.svg"},
  { id: 4, value: "", img: "/assets/corbeau.svg"},
  { id: 5, value: "", img: "/assets/skull.svg"},
  { id: 6, value: "", img: "/assets/witch.svg"}
]

// on duplique chaque carte pour créer les paires
const cards = ref(
  [...baseCards, ...baseCards]
    .map((card, index) => ({ ...card, id: index + 1, flipped: false })) // on donne un id unique
    .sort(() => Math.random() - 0.5) // mélange aléatoire
)

// --- fonction pour retourner une carte ---
const flipCard = (card) => {
  card.flipped = !card.flipped
}
</script>

<template>
  <header>

    <img src="/assets/logo.svg" alt="Logo du jeu"/>
  </header>
  <main>
    <div class="display-container">
      <div
        class="card"
        v-for="card in cards"
        :key="card.id"
        :class="{ flipped: card.flipped }"
        @click="flipCard(card)"
      >
        <!-- DOS -->
        <div class="card-face card-back">
          <img src="/assets/doscarte.svg" alt="Dos de carte" />
        </div>

        <!-- AVANT -->
        <div class="card-face card-front">
          <img :src="card.img" :alt="`Image ${card.value}`" />
          {{ card.value }}
        </div>
      </div>
    </div>
  </main>
</template>

<style>
header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  display: flex;
  justify-content: center;
  padding: 10px 0;
  z-index: 10;
  background: transparent; /* ou une couleur si tu veux */
}

main {
  margin-top: 60px; /* hauteur du header fixe */
  display: flex;
  justify-content: center;
  align-items: flex-start;
}



header img{
 width: 100px;
 align-items: center;
 
}

body {
 background:linear-gradient(90deg,#683200,#ff7332,#e96500);
 animation: gradient 10s ease infinite;
 background-size: 200% 200%;
}
@keyframes gradient {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }

}

main {
  display: flex;
  justify-content: center;
  align-items: flex-start; /* aligne en haut */
  padding-top: 60px; /* espace au dessus */
  margin-top: 0;
}

.display-container {
  display: grid;
  grid-template-columns: repeat(4, 120px);
  gap: 15px;
  justify-content: center;
  margin: 0 auto;
}

.card {
  position: relative;
  width: 120px;
  height: 160px;
  perspective: 1000px; /* permet la 3D */
  cursor: pointer;
}

.card-face {
  background-color: rgb(255, 119, 0);
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 10px;
  border: 3px solid black;
  backface-visibility: hidden; /* cache la face arrière */
  transition: transform 0.6s;
}

.card-back {
  transform: rotateY(0deg);
}

.card-front {
  background-color: rgb(255, 255, 255);
  font-weight: bold;
  font-size: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  transform: rotateY(180deg);
}

.card.flipped .card-back {
  transform: rotateY(180deg);
}

.card.flipped .card-front {
  transform: rotateY(360deg);
}

.card img {
  width: 100%;
  height: 100%;
  border-radius: 10px;
}
</style>
