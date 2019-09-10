<template>
  <div>
    <button @click="active=!active">Click</button>

    <ul class="circle-container" :class="{'active':active}">
      <li>1</li>
      <li>2</li>
      <li>3</li>
      <li>4</li>
      <li>5</li>
      <li>6</li>
      <li>7</li>
      <li>8</li>
      <li>9</li>
      <li>X</li>
    </ul>

    <ul class="circle-container2" :class="{'active':active}" :style="parentClass">
      <li v-for="(item, index) in items" :key="item" :style="itemClass(index)">{{item}}</li>
    </ul>


  </div>
</template>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'

@Component({})
export default class RadialInput extends Vue {
  active = false
  circleSize = 300
  itemSize = 60
  items = [1, 2, 3, 4, 5, 6, 7, 8]

  get angle () {
    return 360 / this.items.length
  }

  get parentClass () {
    return {
      position: 'relative',
      /* 1 */
      width: '20em',
      height: '20em',
      padding: 0,
      borderRadius: '50%',
      listStyle: 'none',
      /* 2 */
      boxSizing: 'content-box',
      /* 3 */
      margin: '5em auto 0',
      border: 'solid 5px tomato'
    }
  }

  get halfParent () {
    return this.circleSize / 2
  }

  itemClass (index) {
    const rotate = index * this.angle;
    const margin = -(this.itemSize / 2);
    return {
      display: 'block',
      position: 'absolute',
      top: '50%',
      left: '50%',
      width: `${this.itemSize}px`,
      height: `${this.itemSize}px`,
      margin: `${margin}px`,
      transform: `rotate(${rotate}deg)  translate(${this.halfParent}px) rotate(${rotate * -1}deg)`,
      background: 'gray'
    }
  }

}
</script>

<style scoped lang="scss">

  /// Mixin to put items on a circle
  /// [1] - Allows children to be absolutely positioned
  /// [2] - Allows the mixin to be used on a list
  /// [3] - In case box-sizing: border-box has been enabled
  /// [4] - Allows any type of direct children to be targeted
  ///
  /// @param {Integer} $nb-items - Number or items
  /// @param {Length} $circle-size - Container size
  /// @param {Length} $item-size - Item size
  /// @param {String | false} $class-for-IE - Base class name for old IE
  @mixin distribute-on-circle(    $nb-items,    $circle-size,    $item-size ) {
    $half-item: ($item-size / 2);
    $half-parent: ($circle-size / 2);

    position: relative; /* 1 */
    width: $circle-size;
    height: $circle-size;
    padding: 0;
    border-radius: 50%;
    list-style: none; /* 2 */
    box-sizing: content-box; /* 3 */

    > * { /* 4 */
      display: block;
      position: absolute;
      top: 50%;
      left: 50%;
      width: $item-size;
      height: $item-size;
      margin: -$half-item;
    }

    $angle: (360 / $nb-items);
    $rot: 0;

    @for $i from 1 through $nb-items {
      > :nth-of-type(#{$i}) {
        transform: rotate($rot * 1deg) translate($half-parent) rotate($rot * -1deg);
      }
      $rot: ($rot + $angle);
    }
  }

  .circle-container {
    @include distribute-on-circle(10, 300px, 50px);
    margin: 5em auto 0;
    border: solid 5px tomato;
  }

  .circle-container li {
    display: flex;
    background: #F5F5F5;
    justify-content: center;
    align-items: center;
    /*display: block;*/
    /*width: 100%;*/
    /*border-radius: 50%;*/
    /*filter: grayscale(100%);*/
    /**/
    /*&:hover {*/
    /*  filter: grayscale(0);*/
    /*}*/
  }


</style>
