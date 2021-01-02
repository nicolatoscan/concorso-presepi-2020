<template>
  <div class="presepe">
    <h3>{{name}}</h3>
    <p class="description" v-if="description">{{description}}</p>
    <ImageSlider :images="images" />
    <StarRating :modelValue="modelValue"  @update:modelValue="$emit('update:modelValue', $event)" :enabled="enabled" />
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
    description: String,
    images: {
      type: Array,
      default: () => []
    },
    modelValue: Number,
    enabled: Boolean
  }
});
</script>

<style scoped lang="scss">
.presepe {
  margin: 2em auto;
  background: radial-gradient(ellipse at bottom, #FF2222 0%, #992222 100%);
  border-radius: 10px;
  padding: 1em;
  border: 1px solid #DDA700;

  h3, .description {
    color: white;
    padding: 0 1em;
  }

}
</style>
