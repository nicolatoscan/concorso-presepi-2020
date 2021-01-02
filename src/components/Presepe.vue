<template>
  <div class="hello">
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
