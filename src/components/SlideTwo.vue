<template>
  <div class="w-full max-w-4xl mx-auto overflow-hidden">
    <div class="relative w-full aspect-video overflow-hidden">
      <img
        src="@/assets/harvest-1.jpg"
        class="absolute inset-0 w-full h-full object-cover view"
      />
      <img
        ref="farmer"
        src="@/assets/harvest-person.png"
        class="absolute bottom-[0%] left-[0%] w-full opacity-0 element farmer"
      />
      <img
        ref="fruit1"
        src="@/assets/harvest-fruit1.png"
        class="absolute bottom-[0%] left-[0%] w-full opacity-0 element fruit-1"
      />
      <img
        ref="fruit2"
        src="@/assets/harvest-fruit2.png"
        class="absolute bottom-[0%] right-[0%] w-full opacity-0 element fruit-2"
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
        Harvesting - Coffee cherries are picked by hand when they turn bright red.
        </div>
    </div>
  </div>
</template>

<script>
import * as anime from 'animejs';

export default {
  name: 'SlideTwo',
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
      anime.set('.farmer', { opacity: 0, translateX: 50 });
      anime.set('.fruit-1', { opacity: 0, translateY: 50 });
      anime.set('.fruit-2', { opacity: 0, translateY: 30 });
    },
    animate() {
      const tl = anime.createTimeline({ autoplay: true });
      tl.add('.view', {
        opacity: [0, 1],
        translateX: [50, 0],
        duration: 100,
      })
      .add(
        '.farmer',
        {
          opacity: [0, 1],
          translateX: [0, 50],
          duration: 1500,
        },
        '-=200',
      )
      .add(
        '.fruit-1',
        {
          opacity: [0, 1],
          translateY: [50, 0],
          duration: 800,
        },
        '-=400',
      )
      .add(
        '.fruit-2',
        {
          opacity: [0, 1],
          translateY: [30, 0],
          duration: 1200,
        },
        '-=600',
      );
    }
  },
};
</script>

<style scoped>
.element {
  opacity: 0;
}

/* ensure view starts visible */
.view {
  opacity: 1;
}
.story {
  opacity: 0;
}
</style>