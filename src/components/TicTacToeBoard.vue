<template>
  <div class="tic-tac-toe-board">
    <div v-for="(_, index) in 9" :key="index" class="cell" @click="onCellClick(index)">
      <div class="cell-content">{{ board[index] }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  board: string[];
  currentPlayer: string;
  onMove: (index: number) => void;
}

const props = defineProps<Props>();

const onCellClick = (index: number) => {
  if (!props.board[index]) {
    props.onMove(index);
  }
};

const boardClasses = computed(() => ({
  'player-x': props.currentPlayer === 'X',
  'player-o': props.currentPlayer === 'O',
}));
</script>

<style scoped>
.tic-tac-toe-board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 4px;
  width: 300px;
  height: 300px;
}

.cell {
  background-color: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  cursor: pointer;
}

.cell-content {
  font-weight: bold;
}

.player-x .cell:hover:empty {
  background-color: rgba(255, 0, 0, 0.2);
}

.player-o .cell:hover:empty {
  background-color: rgba(0, 0, 255, 0.2);
}
</style>
