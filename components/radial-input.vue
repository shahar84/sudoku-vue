<template>
  <div>
    <div style="margin-top: 40px">
      <button @click="active=!active">Click</button>
      {{active}}
    </div>

    <ul class="circle-container2" :class="{'active':active}" :style="parentClass">
      <li v-for="(item, index) in items" :key="item" :style="itemClass(index)">{{item}}</li>
    </ul>


  </div>
</template>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'

const directionOptions = Object.freeze({
  'LEFT': 'LEFT',
  'RIGHT': 'RIGHT',
  'FULL': 'FULL'
})

@Component({})
export default class RadialInput extends Vue {
  active = false
  circleSize = 300
  itemSize = 60
  items = [1, 2, 3, 4, 5, 6, 7, 8]
  direction = directionOptions.FULL

  get angle () {
    let angle;
    switch (this.direction) {
      case directionOptions.LEFT:
        angle = -180;
        break;
      case directionOptions.RIGHT:
        angle = 180;
        break;
      default:
        angle = 360;
    }
    return angle / this.items.length
  }

  get parentClass () {
    return {
      position: 'relative',
      width: `${this.circleSize}px`,
      height: `${this.circleSize}px`,
      padding: 0,
      borderRadius: '50%',
      listStyle: 'none',
      boxSizing: 'content-box',
      margin: '5em auto 0',
      border: 'solid 5px tomato'
    }
  }

  get halfParent () {
    return this.circleSize / 2
  }

  itemClass (index) {
    const rotate = (index * this.angle) - 90
    const margin = -(this.itemSize / 2)
    const translate = this.active ? this.halfParent : 0
    const opacity = this.active ? 1 : 0
    const transform = this.active ?
      `rotate(${rotate}deg)  translate(${translate}px) scale(1) rotate(${rotate * -1}deg)` :
      `rotate(-90deg) scale(0.8)`
    const transition = this.active ? '0.3s all' : '0.5s all'

    return {
      display: 'flex',
      justifyContent: 'center',
      alignItems: 'center',
      position: 'absolute',
      top: '50%',
      left: '50%',
      width: `${this.itemSize}px`,
      height: `${this.itemSize}px`,
      margin: `${margin}px`,
      background: 'gray',
      transform,
      opacity,
      transition,
    }
  }

}
</script>

<style scoped lang="scss">


</style>
