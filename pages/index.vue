<template>
  <div class="container">
    <div>
      <h1 class="title text-4xl">
        sudoku
      </h1>
      <board/>
    </div>
  </div>
</template>

<script>
import Board from '@/components/board'

import Vue from 'vue'
import Component from 'vue-class-component'

@Component({
  components: {
    Board,
    RadialInput
  }
})
export default class Index extends Vue {

}
</script>

<style>
  body {
    background: #4568DC; /* fallback for old browsers */
    background: -webkit-linear-gradient(to right, #B06AB3, #4568DC); /* Chrome 10-25, Safari 5.1-6 */
    background: linear-gradient(to right, #B06AB3, #4568DC); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */

  }

  .container {
    margin: 0 auto;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }
</style>
