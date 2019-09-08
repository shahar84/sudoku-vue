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
import Board from '@/components/board';

export default {
  components: {
    Board
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
