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
  <main
    class="dark:bg-gray-800 min-h-screen dark:text-white text-center flex justify-center items-center flex-col"
  >
    <h2 class="py-12 text-5xl font-bold capitalize">this is XO game</h2>
    <div class="py-4 text-xl">player {{ player }}'s turn</div>
    <section class="flex justify-center items-center">
      <div v-for="(row, x) in board" :key="x" class="">
        <div
          v-for="(col, y) in row"
          :key="y"
          @click="makeMove(x, y)"
          :class="`w-20 h-20 border-white border flex justify-center items-center text-2xl hover:bg-gray-700 cursor-pointer ${
            board[x][y] === 'x' ? 'text-red-500' : 'text-blue-500'
          }`"
        >
          {{ board[x][y] }}
        </div>
      </div>
    </section>
    <section class="py-5">
      <button
        @click="resetGame"
        class="bg-red-500 text-white px-5 py-2 hover:bg-red-600 duration-300 rounded font-bold"
      >
        reset game
      </button>
      <div v-if="winner" class="text-6xl uppercase my-3">
        the winner is {{ winner }}!!
      </div>
    </section>
  </main>
</template>
