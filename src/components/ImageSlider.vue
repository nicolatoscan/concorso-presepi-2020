<template>
<div class="slider-wrapper">
  <div class="slider" ref="slider">
    <img :ref="setImgRefs" v-for="img in images" :key="img" :src="img" />
  </div>
  <div class="controls-bottom">
    <div
      class="control-ball"
      v-for="(n, i) in Array(images.length)"
      :key="i"
      :class="i == currentPosition ? 'current' : ''"
      @click="scrollToPosition(i)"></div>
  </div>
  <div class="controls-arrow controls-arrow-left" @click="scrollToPosition(currentPosition - 1)"><span>&larr;</span></div>
  <div class="controls-arrow controls-arrow-right" @click="scrollToPosition(currentPosition + 1)"><span>&rarr;</span></div>
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
    this.observer = new IntersectionObserver(function (entries) {
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
      if (pos < 0)
        pos = this.$data.imgElements.length + pos
      this.$data.imgElements[pos % this.$data.imgElements.length].scrollIntoView({ behavior: "smooth" })
    }
  }
});
</script>

<style scoped lang="scss">
.slider-wrapper {
  position: relative;
  width: 90vw;
  .slider {
    display: flex;
    flex-direction: row;
    overflow: auto;
    scroll-snap-type: x mandatory;
    &::-webkit-scrollbar {
      display: none;
    }

    img {
      width: 90vw;
      scroll-snap-align: start;
    }
  }

  .controls-arrow {
    position: absolute;
    top: 0;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    font-size: em;
    background-color: rgba(0, 0, 0, 0.3);
    cursor: pointer;
    &.controls-arrow-left {
      left: 0;
    }
    &.controls-arrow-right {
      right: 0;
    }
    &:hover {
      background-color: rgba(0, 0, 0, 0.6);
    }
    .controls-arrow {
      width: 50%;
    }
    span {
      padding: 1em;
    }
  }
  

  .controls-bottom {
    position: absolute;
    bottom: 0.1em;
    width: 100%;
    .control-ball {
      width: 0.5em;
      height: 0.5em;
      margin: 0.3em;
      border-radius: 50%;
      display: inline-block;
      background-color: #888;
      &.current {
        background-color: black;
      }
    }
  }
}

.current {
  color: red;
}
</style>
