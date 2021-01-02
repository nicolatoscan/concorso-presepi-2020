<template>
  <div class="presepe">
    <h2>{{name}}</h2>
    <ImageSlider :images="images" />
    <StarRating :value="modelValue"  @input="$emit('update:modelValue', $event)"/>
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
        'https://cdn.pixabay.com/photo/2015/12/12/15/24/amsterdam-1089646_1280.jpg',
        'https://cdn.pixabay.com/photo/2016/02/17/23/03/usa-1206240_1280.jpg',
        'https://cdn.pixabay.com/photo/2015/05/15/14/27/eiffel-tower-768501_1280.jpg',
        'https://cdn.pixabay.com/photo/2016/12/04/19/30/berlin-cathedral-1882397_1280.jpg',
      ]
    },
    modelValue: Number
  }
});
</script>

<style scoped lang="scss">

</style>
