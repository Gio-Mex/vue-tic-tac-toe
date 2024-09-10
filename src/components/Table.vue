<script setup>
import { ref, reactive } from "vue";

const table = reactive([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const player = ref("X");
const winner = ref("");
const endGame = ref(false);

const $emit = defineEmits(["end-game"]);

// Click function
function onClick(i, j) {
  table[i][j] = player.value;
  if (checkWin()) {
    winner.value = player.value;
    endGame.value = true;
    $emit("end-game", winner.value);
  } else if (table.every((row) => row.every((cell) => cell))) {
    endGame.value = true;
    $emit("end-game", "draw");
  } else {
    player.value = player.value === "X" ? "O" : "X";
  }
}

// Check for win condition
function checkWin() {
  const p = player.value;

  for (let i = 0; i < 3; i++) {
    if (table.every((row) => row[i] === p)) return true;
    if (table[i].every((cell) => cell === p)) return true;
  }

  if (table[0][0] === p && table[1][1] === p && table[2][2] === p) return true;
  if (table[0][2] === p && table[1][1] === p && table[2][0] === p) return true;

  return false;
}

// Reset function
function reset() {
  table.forEach((row) => row.fill(""));
  player.value = "X";
  winner.value = "";
  endGame.value = false;
  $emit("end-game");
}
</script>

<template>
  <div class="table" :class="{ disabled: endGame }">
    <div v-for="(row, i) in table" :key="i" class="row">
      <div
        v-for="(cell, j) in row"
        :key="j"
        class="cell"
        :class="[{ X: cell === 'X' }, { O: cell === 'O' }, { disabled: cell !== '' }]"
        @click="onClick(i, j)"
      >
        {{ cell }}
      </div>
    </div>
  </div>

  <button @click="reset">Reset board</button>
</template>

<style scoped>
.table {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin: 10px;
  margin-bottom: 20px;
}

.row {
  display: flex;
  flex-direction: row;
  gap: 8px;
}

.cell {
  width: 100px;
  height: 100px;
  border: 2px solid gray;
  border-radius: 5px;
  font-size: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.cell:hover {
  background-color: whitesmoke;
}

.cell:active {
  background-color: gainsboro;
  translate: 0px 3px;
}

button:hover {
  background-color: black;
}

button:active {
  translate: 0px 3px;
}

h1 {
  font-size: 2.5em;
}

.disabled {
  pointer-events: none;
  opacity: 0.8;
}
</style>
