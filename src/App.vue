<script setup lang="ts">
import { ref, shallowRef } from 'vue';
import TicTacToeBoard from './components/TicTacToeBoard.vue';

const board = ref<string[]>(Array(9).fill(''));
const currentPlayer = ref<'X' | 'O'>('X');
const winner = shallowRef<string | null>(null);

const winningCombinations = [
  [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
  [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
  [0, 4, 8], [2, 4, 6] // Diagonals
];

const makeMove = (index: number) => {
  if (!winner.value && !board.value[index]) {
    board.value[index] = currentPlayer.value;
    checkWinner();
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
  }
};

const checkWinner = () => {
  for (const combination of winningCombinations) {
    const [a, b, c] = combination;
    if (
      board.value[a] &&
      board.value[a] === board.value[b] &&
      board.value[a] === board.value[c]
    ) {
      winner.value = board.value[a];
      return;
    }
  }
  if (board.value.every(cell => cell !== '')) {
    winner.value = 'T'; // Tie
  }
};

const resetGame = () => {
  board.value = Array(9).fill('');
  currentPlayer.value = 'X';
  winner.value = null;
};
</script>

<template>
  <div class="app">
    <h1>Tic-Tac-Toe</h1>
    <TicTacToeBoard
      :board="board"
      :currentPlayer="currentPlayer"
      @move="makeMove"
    />
    <div v-if="winner" class="winner-message">
      {{ winner === 'T' ? 'It\'s a tie!' : `${winner} wins!` }}
    </div>
    <button @click="resetGame" class="reset-button">Reset Game</button>
  </div>
</template>


<style scoped>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, sans-serif;
}

.winner-message {
  margin-top: 1rem;
  font-size: 1.5rem;
  font-weight: bold;
}

.reset-button {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  font-size: 1rem;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.reset-button:hover {
  background-color: #45a049;
}
</style>
