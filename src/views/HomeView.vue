<script lang="ts" setup>
import { computed, reactive, ref, unref, watch } from "vue";
const player = ref<"x" | "o">("x");

const board = reactive<string[][]>([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);
const calculateWinner = (squares: string[]) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
};
const winner = computed(() => calculateWinner(unref(board).flat()));
const makeMove = (xP: number, yP: number) => {
  const currentCell = board[xP][yP];
  if (currentCell) return;
  if (winner.value) {
    resetGame();
    return;
  }
  board[xP][yP] = player.value;
  player.value = player.value === "x" ? "o" : "x";
};
const resetGame = () => {
  board.map((item) => item.fill(""));
  player.value = "x";
};
watch([winner, board], ([winnerVal, boardVal]) => {
  const isBoardFull = boardVal.flat().every((cell) => cell !== "");
  if (!winnerVal && isBoardFull) resetGame();
});
</script>
<template>
  <div class="home text-red-500">this is home page</div>
</template>
