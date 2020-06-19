<template>
  <div class="home">
    <knob max="360" :value="knob" size="700px" @input="(value) => knob = value" label="Titulo"/>
    <knob max="360" :value="knob" size="500px" @input="(value) => knob = value" label="Titulo"/>
    <knob max="360" :value="knob" size="300px" @input="(value) => knob = value" label="Titulo"/>
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
      knob: 90
    }
  }
};
</script>
