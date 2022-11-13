<script setup>
import { ref, computed } from 'vue'

const player = ref(null)
const winner = ref(null)
const cells = ref(null)

const winPatterns = [
  [
    [0, 0],
    [0, 1],
    [0, 2],
  ],
  [
    [1, 0],
    [1, 1],
    [1, 2],
  ],
  [
    [2, 0],
    [2, 1],
    [2, 2],
  ],
  [
    [0, 0],
    [1, 0],
    [2, 0],
  ],
  [
    [0, 1],
    [1, 1],
    [2, 1],
  ],
  [
    [0, 2],
    [1, 2],
    [2, 2],
  ],
  [
    [0, 0],
    [1, 1],
    [2, 2],
  ],
  [
    [0, 2],
    [1, 1],
    [2, 0],
  ],
]

const newGame = () => {
  player.value = Math.round(Math.random() * 1) === 1 ? 'x' : 'o'
  winner.value = null
  cells.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ]
}

newGame()

const tic = (row, col) => {
  if (winner.value || cells.value[row][col]) return

  cells.value[row][col] = player.value
  checkWinner()

  player.value = player.value === 'x' ? 'o' : 'x'
}

const checkWinner = () => {
  winPatterns.forEach(pattern => {
    const verify = []
    pattern.forEach(cell => {
      const [i, j] = cell
      verify.push(cells.value[i][j])
    })

    if (verify.every(cell => cell != '' && cell === verify[0])) {
      winner.value = verify[0]
    }
  })
}
</script>

<template>
  <div class="w-full h-full flex flex-col justify-center items-center gap-5">
    <h1 class="text-2xl">Tic-tac-toe</h1>
    <div class="grid grid-cols-3 gap-5">
      <template v-for="(row, i) in cells">
        <template v-for="(cell, j) in row">
          <button @click="tic(i, j)" class="w-10 h-10 border">{{ cell }}</button>
        </template>
      </template>
    </div>
    <p v-if="winner">Player {{ winner }} win!</p>
    <p v-else>Player's {{ player }} turn</p>
    <button @click="newGame()" class="border rounded p-1">retry</button>
  </div>
</template>
