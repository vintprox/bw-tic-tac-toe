<template>
  <div :class="$style.field">
    <div
      v-for="(cell, cellIndex) in field"
      :key="cellIndex"
      :class="$style.cell"
      @click="makeTurn(cellIndex)"
    >
      {{ cell === 0 ? '-' : cell === 1 ? 'X' : 'O' }}
    </div>
  </div>
</template>

<script>
const fieldSize = 3

export default {
  data: () => ({
    gameOn: true,
    /**
     * Array representing the cells of the tic-tac-toe field
     *
     * Cell values:
     * * 0 -
     * * 1 - X
     * * 2 - O
     */
    field: new Array(fieldSize ** 2).fill(0),
    combinations: [],
    playerNumber: 1
  }),
  methods: {
    getPositionByIndex (index) {
      const y = Math.floor(index / fieldSize)
      const x = index % fieldSize
      return [y, x]
    },
    getIndexByPosition (y, x) {
      return y * fieldSize + x
    },
    getCellByPosition (y, x) {
      return this.field[this.getIndexByPosition(y, x)]
    },
    makeTurn (cellIndex) {
      if (!this.gameOn || this.field[cellIndex] !== 0) return

      this.$set(this.field, cellIndex, this.playerNumber)

      for (let combination of this.combinations) {
        let playerOccupying = this.field[combination[0]]
        if (playerOccupying === 0) continue

        if (combination.every(index => this.field[index] === playerOccupying)) {
          this.gameOn = false
          this.$emit('win', playerOccupying)
          return
        }
      }

      this.playerNumber = this.playerNumber === 1 ? 2 : 1
    }
  },
  created () {
    const primaryDiagonalCombination = new Array(fieldSize).fill(null).map((_, index) => index * (fieldSize + 1));
    const secondaryDiagonalCombination = new Array(fieldSize).fill(null).map((_, index) => (index + 1) * fieldSize);
    this.combinations.push(primaryDiagonalCombination);
    this.combinations.push(secondaryDiagonalCombination);
    for (let size = 0; size < fieldSize; size++) {
      const rowFirstIndex = size * fieldSize
      const rowCombination = new Array(fieldSize).fill(null).map((_, columnIndex) => rowFirstIndex + columnIndex)
      const columnCombination = new Array(fieldSize).fill(null).map((_, columnIndex) => size + columnIndex * fieldSize)
      this.combinations.push(rowCombination);
      this.combinations.push(columnCombination);
    }
    console.log(this.combinations)
  }
}
</script>

<style module>
.field {
  display: flex;
  box-sizing: border-box;
  flex-wrap: wrap;
  width: 300px;
  background-color: black;
  border-collapse: collapse;
}

.cell {
  box-sizing: border-box;
  height: 100px;
  width: 100px;
  border: 4px solid #aaa;
  font-size: 48px;
  background-color: white;
  color: black;
}
</style>
