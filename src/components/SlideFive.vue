<template>
  <div class="w-full max-w-4xl mx-auto overflow-hidden">
    <div class="relative w-full aspect-video overflow-hidden">
      <img
        src="@/assets/roasting-1.jpg"
        class="absolute inset-0 w-full h-full object-cover view"
      />
      <img
        ref="smoke-1"
        src="@/assets/roasting-2.png"
        class="absolute top-[0%] left-[0%] w-full opacity-0 element smoke-1"
      />
      <img
        ref="smoke-2"
        src="@/assets/roasting-3.png"
        class="absolute top-[0%] left-[0%] w-full opacity-0 element smoke-2"
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
        Roasting - Beans are roasted at high temperature to develop flavor.
      </div>
    </div>
  </div>
</template>

<script>
import * as anime from 'animejs';

export default {
  name: 'SlideFour',
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
      anime.set('.smoke-1', { opacity: 0, translateX: -50 });
      anime.set('.smoke-2', { opacity: 0, translateY: -50 });
    },
    animate() {
      const tl = anime.createTimeline({ autoplay: true });
      tl.add('.view', {
        opacity: [0, 1],
        translateY: [50, 0],
        duration: 100,
      })
        .add(
          '.smoke-1',
          {
            opacity: [0, 1],
            translateY: [-50, 0],
            duration: 1200,
          },
          '-=600',
        )
        .add(
          '.smoke-2',
          {
            opacity: [0, 1],
            translateY: [50, 0],
            duration: 1600,
          },
          '-=800',
        );
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
