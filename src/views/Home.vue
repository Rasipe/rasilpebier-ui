<template>
  <div class="home">
    <knob max="1000" :value="knob" @input="(value) => knob = value"/>
  </div>
</template>

<script>
import Knob from "@/components/Knob.vue";

export default {
  name: "Home",
  components: {
    Knob
  },
  data() {
    return {
      knob: 250
    }
  }
};
</script>
