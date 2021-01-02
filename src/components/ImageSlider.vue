<template>
  <div class="slider carousel" ref="slider">
    <img class="ciao" :ref="setImgRefs" v-for="img in images" :key="img" :src="img" />
  </div>
  <div>
    <span
      v-for="(n, i) in Array(images.length)"
      :key="i"
      :class="i == currentPosition ? 'current' : ''"
      @click="scrollToPosition(i)">{{i}} - </span>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "ImageSlider",
  data() {
    return {
      images: [
        "https://cdn.pixabay.com/photo/2015/12/12/15/24/amsterdam-1089646_1280.jpg",
        "https://cdn.pixabay.com/photo/2016/02/17/23/03/usa-1206240_1280.jpg",
        "https://cdn.pixabay.com/photo/2015/05/15/14/27/eiffel-tower-768501_1280.jpg",
        "https://cdn.pixabay.com/photo/2016/12/04/19/30/berlin-cathedral-1882397_1280.jpg",
      ],
      imgElements: [] as HTMLElement[],
      currentPosition: 0,
      observer: null as IntersectionObserver | null
    };
  },
  mounted: function () {
    console.log(this.imgElements.length)

    const setCurrentPosition = (url: string) => {
      const i = this.images.indexOf(url)
      if (i >= 0)
        this.currentPosition = i
      console.log(this.currentPosition)
    }
    this.observer = new IntersectionObserver(function (entries, observer) {
        const activated = entries.reduce(function (max, entry) {
          return entry.intersectionRatio > max.intersectionRatio ? entry : max;
        });
        if (activated.intersectionRatio > 0) {
          setCurrentPosition(activated.target.getAttribute("src") ?? '');
        }
      },
      {
        root: this.$refs["slider"] as Element,
        threshold: 0.5,
      }
    );

    for (let i = 0; i < this.$data.imgElements.length; i++) {
      this.observer.observe(this.imgElements[i]);
    }

  },
  beforeUpdate() {
    this.imgElements = []
  },
  methods: {
    setImgRefs(el: HTMLElement) {
      if (el) {
        this.imgElements.push(el)
        if (this.observer)
          this.observer.observe(el)
      }
    },
    scrollToPosition: function (pos: number) {
      this.$data.imgElements[pos % this.$data.imgElements.length].scrollIntoView({ behavior: "smooth" })
    }
  }
});
</script>

<style scoped lang="scss">
.slider {
  display: flex;
  flex-direction: row;
  width: 90vw;
  overflow: auto;
  scroll-snap-type: x mandatory;

  img {
    width: 90vw;
    scroll-snap-align: start;
  }
}

.current {
  color: red;
}
</style>
