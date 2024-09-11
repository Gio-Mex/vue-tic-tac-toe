<script setup>
import Table from "./components/Table.vue";

import { ref } from "vue";

let player = ref("");
let endGame = ref(false);
let xCount = ref(0);
let oCount = ref(0);
let drawCount = ref(0);

//End game handler
function handleEndGame(winner) {
  player.value = winner;
  player.value ? (endGame.value = true) : (endGame.value = false);

  // Stats counters
  if (winner === "X") {
    xCount.value++;
  } else if (winner === "O") {
    oCount.value++;
  } else if (winner === "draw") {
    drawCount.value++;
  }
}

function resetPoints() {
  xCount.value = 0;
  oCount.value = 0;
  drawCount.value = 0;
}
</script>

<template>
  <div class="app">
    <h1>Tic Tac Toe</h1>

    <div class="stats">
      <h2>Stats</h2>
      <div class="counter">
        <p>Player <span class="X">X</span>: {{ xCount }}</p>
        <p><span style="color: green">Draws</span>: {{ drawCount }}</p>
        <p>Player <span class="O">O</span>: {{ oCount }}</p>
      </div>
      
    </div>
    <button @click="resetPoints">Reset points</button>

    <h2 v-if="endGame && player !== 'draw'">
      🎉 Player
      <span :class="player === 'X' ? 'X' : 'O'">{{ player }}</span> wins! 🎉
    </h2>
    <h2 v-else-if="endGame && player === 'draw'">🤝 It's a draw! 🤝</h2>
    <h2 v-else>🎲 Game in progress... 🎲</h2>

    <Table @end-game="handleEndGame"></Table>
    
  </div>
</template>

<style scoped>
.app {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

h1 {
  color: darkslategray;
  font-size: 3.5em;
  margin: 15px 0;
}

.stats {
  display: flex;
  flex-direction: column;
  text-align: center;
}

.counter {
  display: flex;
  gap: 40px;
}

h2 {
  color: #3e3e3e;
  font-size: 1.6em;
}

p {
  color: #3e3e3e;
  font-size: 1.4em;
  margin-top: 0;
}
</style>
