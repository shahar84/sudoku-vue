<template>
  <div class="section">
    <h2 class="text-2xl" v-if="isSolved">You solved it!</h2>
    <section class="board">
      <div v-for="(row, index) in puzzle" :key="index" class="board__row">
        <div v-for="(item, column) in row" :key="`${index}_${column}`"
             class="board__column border border-gray-400"
             :class="{'bg-gray-300':item.readonly,
             'bg-yellow-300': helpNumber === item.value && item.value!== null}">
          <input type="number"
                 @keypress="isNumber"
                 v-model.number="puzzle[index][column].value"
                 :readonly="item.readonly"
                 :disabled="item.readonly">

        </div>
      </div>
    </section>
    <button @click="cheat">cheat</button>
    <div>
      <button
        class="rounded w-8 rounded-full
        border-gray-400 border pt-1 pb-1 pr-2 pl-2 m-2"
        v-for="num in helperNumberOptions"
        :class="{'bg-yellow-300': helpNumber === num, 'bg-blue-200': helpNumber !== num}"
        :key="num" @click="boldNumber(num)">{{num}}
      </button>
    </div>

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
  helperNumberOptions = Array.from({ length: 9 }, (v, k) => k + 1)
  helpNumber = null

  mounted () {
    this.newGame()
  }

  boldNumber (num) {
    this.helpNumber = this.helpNumber === parseInt(num) ? null : parseInt(num)
  }

  newGame () {
    const rawPuzzle = makepuzzle()

    const rawSolvePuzzle = solvepuzzle(rawPuzzle)
    const fixedSolvePuzzle = rawSolvePuzzle.map(fixPuzzle)
    const fixedPuzzle = rawPuzzle.map(fixPuzzle)
    this.puzzle = generatePuzzle(fixedPuzzle)
    this.solvePuzzle = fixedSolvePuzzle
  }

  isNumber (event) {
    const value = event.target.value;
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
      grid-template-columns: repeat(9, 1fr);
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
    @media only screen and (max-width: 600px)  {
      height: 30px;
      width: 30px;
      font-size: 1.2rem;

    }

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
