<template>
  <div class="section">
    {{isSolved}}
    <section class="board">
      <div v-for="(row, index) in puzzle" :key="index" class="board__row">
        <div v-for="(value, column) in row" :key="`${index}_${column}`"
             class="board__column border border-gray-400"
             :class="{'bg-gray-300':value.readonly}">
          <input type="number"
                 @keypress="isNumber"
                 v-model="puzzle[index][column].value"

          >
          <!--          :readonly="value.readonly"-->
          <!--          :disabled="value.readonly"-->
        </div>
      </div>
    </section>
    <button @click="cheat">cheat</button>
  </div>
</template>
<script>
import Vue from 'vue'
import Component from 'vue-class-component'
import { makepuzzle, solvepuzzle } from 'sudoku'
import isEqual from 'lodash.isequal'

const fixPuzzle = item => item === null ? null : item + 1
const rawPuzzle = [null, 8, 2, null, 7, null, 1, null, null, 1, 4, null, null, null, null, null, null, null, null, null, null, 5, null, 6, null, null, null, 3, 7, null, null, null, 0, null, null, null, null, null, null, 7, null, null, null, 6, null, null, null, null, 1, null, null, 3, null, null, null, 6, null, null, null, null, null, 0, 2, null, 0, null, 6, null, 4, null, null, null, null, 5, 4, null, 0, null, null, null, null]

const generatePuzzle = (arr) => {
  const puzzle = []
  const rawPuzzle = [...arr]
  for (let i = 0; i < 9; i++) {
    const row = rawPuzzle.splice(0, 9).map(item => {
      return {
        value: item,
        readonly: Number.isInteger(item)
      }
    })
    puzzle.push(row)
  }
  return puzzle
}
const puzzle = generatePuzzle(rawPuzzle)

@Component({})
export default class Board extends Vue {
  puzzle = null
  solvePuzzle = null

  mounted () {
    // const rawPuzzle = makepuzzle()
    const rawSolvePuzzle = solvepuzzle(rawPuzzle)
    const fixedSolvePuzzle = rawSolvePuzzle.map(fixPuzzle)
    const fixedPuzzle = rawPuzzle.map(fixPuzzle)
    this.puzzle = generatePuzzle(fixedPuzzle)
    this.solvePuzzle = fixedSolvePuzzle
    console.log(this.solvePuzzle)
  }

  isNumber (event) {
    const value = event.target.value
    const re = new RegExp('^([1-9])$')
    if (re.test(value)) {
      return event.preventDefault()
    }
  }

  cheat () {
    this.puzzle = generatePuzzle(this.solvePuzzle)
  }

  get isSolved () {
    if (!this.solvePuzzle || !this.puzzle) {
      return false
    }
    const puzzle = this.puzzle.flat().map(item => item.value ? parseInt(item.value) : null)
    return isEqual(this.solvePuzzle, puzzle)
  }
}


</script>
<style scoped lang="scss">
  .board {
    display: block;
    text-align: center;
    border: 2px solid black;


    .board__row {
      display: grid;
      grid-template-columns: repeat(9, 60px);
      box-sizing: border-box;
      position: relative;

      &:nth-child(4), &:nth-child(7) {
        &:after {
          content: "";
          position: absolute;
          width: 100%;
          height: 2px;
          background: black;
          display: block;
        }
      }


      .board__column {
        box-sizing: content-box;

        &:nth-child(3), &:nth-child(6) {
          border-right: 2px solid black;
        }
      }
    }
  }


  input {
    background: transparent;
    text-align: center;
    height: 60px;
    width: 60px;
    font-size: 2rem;
    border: none;
  }

  input:read-only {
    user-select: none;
  }

  input[type=number]::-webkit-inner-spin-button,
  input[type=number]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
</style>
