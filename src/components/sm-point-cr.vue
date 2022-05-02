<template lang="html">
  <circle :cx="cx" :cy="cy" :r="r" />
</template>

<script>
import * as math from "mathjs";
export default {
  data: function() {
    return {
      radius: this.$parent.radius,
    };
  },
  props: {
    h: [Number],
    angle: [Number],
    r: {
      type: [Number, String],
      default: 5,
    },
  },
  computed: {
    // a and b calculations taken from:
    // https://www.allaboutcircuits.com/technical-articles/mathematical-construction-and-properties-of-the-smith-chart/
    cx: function() {
      //1 +10i
      let h = this.h;
      let angle = this.angle;
      let ca = h * this.radius * math.cos((angle / 180) * math.pi);
      return ca + this.radius;
    },
    cy: function() {
      let h = this.h;
      let angle = this.angle;
      let co = h * math.sin((angle / 180) * math.pi);
      return this.radius - co * this.radius;
    },
  },
};
</script>
