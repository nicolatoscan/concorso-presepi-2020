<template>
  <div class="presepe">
    <h2>{{name}}</h2>
    <ImageSlider :images="images" />
    <StarRating :modelValue="modelValue"  @update:modelValue="$emit('update:modelValue', $event)"/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import ImageSlider from "@/components/ImageSlider.vue";
import StarRating from "@/components/StarRating.vue";

export default defineComponent({
  name: 'Presepe',
  components: {
    ImageSlider,
    StarRating
  },
  props: {
    name: String,
    images: {
      type: Array,
      default: () => [
        'https://picsum.photos/800/600?1',
        'https://picsum.photos/800/600?2',
        'https://picsum.photos/600/800?3',
        'https://picsum.photos/800/600?4'
      ]
    },
    modelValue: Number
  }
});
</script>

<style scoped lang="scss">
.presepe {
  margin: 2em auto;
  background: radial-gradient(ellipse at bottom, #FF2222 0%, #992222 100%);
  border-radius: 10px;
  padding: 1em;

}
</style>
