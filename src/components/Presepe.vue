<template>
  <div class="presepe box">
    <h3>{{name}}</h3>
    <p class="description" v-if="description">{{description}}</p>
    <ImageSlider :images="images" :isvideo="isvideo" />
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
    isvideo: Boolean,
    modelValue: Number,
    enabled: Boolean
  }
});
</script>

<style scoped lang="scss">
.presepe {
  margin: 2em auto;
  border-radius: 10px;
  padding: 1em;

  h3, .description {
    color: white;
    padding: 0 1em;
  }

}
</style>
