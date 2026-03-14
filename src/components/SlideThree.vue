<template>
  <div
    class="w-full max-w-4xl mx-auto overflow-hidden"
    >
    <div class="relative w-full aspect-video overflow-hidden">
      <img
        src="@/assets/processing-1.jpg"
        class="absolute inset-0 w-full h-full object-cover view"
      />
      <img
        ref="fruits"
        src="@/assets/processing-2.png"
        class="absolute bottom-[-6%] left-[-6%] w-full opacity-0 element fruits"
      />
      <img
        ref="farmer2"
        src="@/assets/processing-3.png"
        class="absolute bottom-[-6%] left-[-5%] w-full opacity-0 element machine"
      />
      <img
        ref="basket2"
        src="@/assets/processing-4.png"
        class="absolute bottom-[-6%] left-[-5%] w-full opacity-0 element complete"
      />
    </div>
    <div v-if="!storyPlaying" class="step-container">
        <button
            @click="$emit('next')"
            class="mt-6 cursor-pointer"
        >
            <img src="@/assets/icon-next.png" alt="Next"/>
        </button>
        <div class="description" ref="description">
            Processing - The beans are separated from the cherry using wet or dry methods.
        </div>
    </div>
    </div>
</template>

<script>
import * as anime from 'animejs';

export default {
  name: 'SlideThree',
  props: {
    active: {
      type: Boolean,
      default: false
    }
  },
  mounted() {
    if (this.active) {
      this.animate();
    }
  },
  watch: {
    active(val) {
      if (val) {
        this.animate();
      } else {
        this.reset();
      }
    }
  },
  methods: {
    reset() {
      // Reset elements to initial state
      anime.set('.fruits', { opacity: 0, translateX: 50 });
      anime.set('.machine', { opacity: 0, translateY: 50 });
      anime.set('.complete', { opacity: 0, translateY: 30 });
    },
    animate() {
      const tl = anime.createTimeline({ autoplay: true });
      tl.add('.view', {
        opacity: [0, 1],
        translateX: [50, 0],
        duration: 100,
      })
      .add(
        '.fruits',
        {
          opacity: [0, 1],
          translateY: [50, 0],
          duration: 2000,
        },
        '-=800',
      )
      .add(
        '.machine',
        {
          opacity: [0, 1],
          translateY: [50, 0],
          duration: 800,
        },
        '-=400',
      )
      .add(
        '.complete',
        {
          opacity: [0, 1],
          translateY: [50, 0],
          duration: 1200,
        },
        '-=600',
      )
    },
  },
};
</script>

<style scoped>
.element {
  opacity: 0;
}
.story {
  opacity: 0;
}
</style>