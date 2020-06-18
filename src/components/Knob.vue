<template>
<div ref="knob" class="knob">
  <svg
    focusable="false"
    viewBox="55.55555555555556 55.55555555555556 111.11111111111111 111.11111111111111"
    class="knob"
    style="transform: rotate3d(0, 0, 1, -90deg);"
    @click="someMethod"
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
    max: [String, Number]
  },
  computed: {
    dashoffset() {
      return -314.1592653589793 / (this.max / this.value);
    }
  },
  methods: {
    someMethod(event) {
      let bodyRect = document.body.getBoundingClientRect(),
      elemRect = event.target.getBoundingClientRect()

      let posX = event.clientX - elemRect.left - bodyRect.left
      let posY = event.clientY - elemRect.top - bodyRect.top

      let centerY = this.$refs.knob.offsetHeight / 2
      let centerX = this.$refs.knob.offsetWidth / 2
      //let angleDeg = Math.atan2(posY - this.$refs.knob.offsetHeight / 2 , posX - this.$refs.knob.offsetWidth / 2 ) * 180 / Math.PI;
      
      /*var deltaX = centerX - posX;
      var deltaY = centerY - posY;
      var rad = Math.atan2(deltaY, deltaX); // In radians
      var angleDeg = rad * (180 / Math.PI)*/
      let side1 = centerY
      let side2 = Math.sqrt( Math.pow((centerX-posX), 2) + Math.pow((centerY - posY), 2) );
      let side3 = Math.sqrt( Math.pow((centerX-posX), 2) + Math.pow(posY, 2) );

      let cos = (Math.pow(side1, 2) + Math.pow(side2, 2) - Math.pow(side3, 2)) / (2 * side1 * side2)
      let angle = Math.acos(cos) * (180 / Math.PI)
      let angleDeg = posX < centerY? angle*2: angle

      let value = angleDeg * (this.max / 360)
      console.log('angulo: '+angleDeg);
      console.log('valor: '+value);
      
      this.$emit('input', value)
      /*
      console.log(event.clientX); // x coordinate
      console.log(event.clientY); // y coordinate

      // pageX/Y gives the coordinates relative to the <html> element in CSS pixels.
      console.log(event.pageX);
      console.log(event.pagey);

      // screenX/Y gives the coordinates relative to the screen in device pixels.
      console.log(event.screenX);
      console.log(event.screenY);
      */
      
    }
  }
};
</script>

<style lang="sass" scoped>
@import '@/assets/style/colors.sass'

.knob
  width: 200px
  height: 200px
.empty
  stroke: grey
.value
  stroke: $primary
</style>