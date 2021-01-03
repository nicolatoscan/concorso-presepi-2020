<template>
<div class="star-wrapper">
  <div v-for="n in [1,2,3,4,5]"
    :key="n"
    class="star"
    :class="modelValue >= n ? 'active' : ''"
    @click="setRating(n)">&#9733;</div>
</div>

</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'StarRating',
  props: {
    modelValue: Number,
    enabled: Boolean,
  },
  methods: {
    setRating: function(r: number) {
      if (this.$props.enabled) {
          if (r < 1)
          r = 1
        if (r > 5)
          r = 5
        this.$emit('update:modelValue', r);
      }
    }
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.star-wrapper {
  display: flex;
  justify-content: center;

  .star {
    padding: 0.3em;
    font-size: 3em;
    color: #999;
    cursor: pointer;
    transition: 0.2s ease-in color;
    font-size: 10vw;
    @media screen and (min-width: 600px) {
      font-size: 4em;
    }
    &.active {
      color: #FFD700;
    }
  }
}

</style>
