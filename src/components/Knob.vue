<template>
<div ref="knob" class="knob" :style="{height: size, width: size}">
  <div class="knob-html">
    <p :style="{height: getSize(6), width: getSize(2), 'font-size': getSize(7)}">{{ label }}</p>
    <input type="number" :value="value" :style="{height: getSize(6), width: getSize(2), 'font-size': getSize(9)}" @input="inputHandler">
  </div>
  <svg
    focusable="false"
    viewBox="55.55555555555556 55.55555555555556 111.11111111111111 111.11111111111111"
    class="knob"
    style="transform: rotate3d(0, 0, 1, -90deg);"
  >
    <circle
      class="value"
      fill="transparent"
      stroke-width="11.111111111111112"
      cx="111.11111111111111"
      cy="111.11111111111111"
      r="50"
      style="transition: stroke-dashoffset 0.6s ease 0s, stroke 0.6s ease 0s;"
    />
    <circle
      fill="transparent"
      class="empty"
      stroke-dasharray="314.159"
      :stroke-dashoffset="dashoffset"
      stroke-width="11.111111111111112"
      cx="111.11111111111111"
      cy="111.11111111111111"
      r="50"
      style="transition: stroke-dashoffset 0.6s ease 0s, stroke 0.6s ease 0s;"
    />
  </svg>
</div>
</template>

<script>
export default {
  name: "Knob",
  props: {
    value: [String, Number],
    max: [String, Number],
    size: String,
    label: String
  },
  computed: {
    dashoffset() {
      let value = parseInt(this.value) > parseInt(this.max)? this.max: this.value
      return  -314.1592653589793 / (this.max / value)
    },
    getSize() {
      return value => {
        let number = this.size.toLowerCase().replace(/[a-z]*/gi, '')
        return parseInt(number) / value + this.size.toLowerCase().replace(/\d*/, '')
      }
    }
  },
  methods: {
    inputHandler(event) {
      let value = event.target.value
      this.$emit('input', value)
    },
    someMethod(event) {
      let bodyRect = document.body.getBoundingClientRect(),
      elemRect = event.target.getBoundingClientRect()

      let posX = event.pageX - elemRect.left - bodyRect.left
      let posY = event.pageY - elemRect.top - bodyRect.top

      let centerY = this.$refs.knob.offsetHeight / 2
      let centerX = this.$refs.knob.offsetWidth / 2
      
      let maxAngle = 360
      let halfAngle = maxAngle / 2

      let deltaY = centerY - posY;
      let deltaX = centerX - posX;
      let rad = Math.atan2(deltaY, deltaX) - 0.45*Math.PI
      let angle = Math.round(rad * halfAngle / Math.PI);

      let angleDeg = angle < 0? angle + 360: angle

      let value = angleDeg * (this.max / maxAngle)
      
      this.$emit('input', value)      
    }
  }
};
</script>

<style lang="sass" scoped>
@import '@/assets/style/colors.sass'

.knob
 position: relative 
.empty
  stroke: grey
.value
  stroke: $primary
.knob-html
  z-index: 10
  position: absolute
  bottom: 40%
  left: 25%
  & > input
    text-align: center
    border: 0px
    background-color: transparent
  & > p
    margin: 0px
  
</style>