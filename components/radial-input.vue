<template>
  <section>
    <div class="radial-input-wrapper" :style="buttonWrapperStyle">
      <button @click="active=!active" class="activation-button">
        <slot>+</slot>
      </button>
      <ul class="circle-container" :style="parentStyle">
        <li v-for="(item, index) in items" :key="item"
            :style="itemStyle(index)" class="item">
          <button @click="handleClick">
            {{item}}
          </button>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'

const directionOptions = Object.freeze({
  'LEFT': 'LEFT',
  'RIGHT': 'RIGHT',
  'FULL': 'FULL'
})

@Component({
  props: {
    open: { type: Boolean, default: undefined }
  }
})
export default class RadialInput extends Vue {
  active = false
  isOpen = false
  circleSize = 300
  itemSize = 60
  buttonSize = 60
  items = [1, 2, 3, 4, 5, 6, 7, 8, 9]
  direction = directionOptions.FULL
  manualMode

  mounted () {
    this.manualMode = typeof this.open !== 'undefined'
    document.addEventListener('click', this.closeMenuEvent)
  }

  beforeDestroy () {
    document.removeEventListener('click', this.closeMenuEvent)
  }

  get shouldOpen () {
    const { open, manualMode, isOpen } = this
    return manualMode ? open : isOpen
  }

  toggleMenu () {
    if (this.manualMode) {
      return
    }
    if (this.isOpen) {
      this.isOpen = false
      this.$emit('close')
    } else {
      this.isOpen = true
      this.$emit('open')
    }
  }

  handleClick () {
    this.$emit('click')
    this.toggleMenu()
  }

  get angle () {
    let angle
    switch (this.direction) {
      case directionOptions.LEFT:
        angle = -180
        break
      case directionOptions.RIGHT:
        angle = 180
        break
      default:
        angle = 360
    }
    return angle / this.items.length
  }

  get buttonWrapperStyle () {
    return {
      width: `${this.buttonSize}px`,
      height: `${this.buttonSize}px`,
    }
  }

  get parentStyle () {
    const pointerEvents = this.active ? 'auto' : 'none'
    return {
      position: 'absolute',
      width: `${this.circleSize}px`,
      height: `${this.circleSize}px`,
      padding: 0,
      borderRadius: '50%',
      listStyle: 'none',
      boxSizing: 'content-box',
      border: 'solid 5px tomato',
      pointerEvents
    }
  }

  get halfParent () {
    return this.circleSize / 2
  }

  itemStyle (index) {
    const rotate = (index * this.angle) - 90
    const margin = -(this.itemSize / 2)
    const translate = this.active ? this.halfParent : 0
    const opacity = this.active ? 1 : 0
    const transform = this.active ?
      `rotate(${rotate}deg)  translate(${translate}px) scale(1) rotate(${rotate * -1}deg)` :
      `rotate(-90deg) scale(0.8)`
    const transition = this.active ? '0.3s all' : '0.5s all'
    const pointerEvents = this.active ? 'auto' : 'none'

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
      borderRadius: '50%',
      transform,
      opacity,
      transition,
      pointerEvents
    }
  }

}
</script>

<style scoped lang="scss">
  .radial-input-wrapper {
    position: relative;
    background: #F5F5F5;
    display: flex;
    justify-content: center;
    align-items: center;
    button.activation-button {
      background: #35495e;
      width: 100%;
      height: 100%;
    }
  }

  .item /deep/ button {
    position: relative;
    display: block;
    width: 100%;
    height: 100%;
  }

</style>
