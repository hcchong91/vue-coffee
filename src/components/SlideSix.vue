<template>
  <div class="w-full max-w-4xl mx-auto overflow-hidden">
    <div
        class="w-full max-w-4xl mx-auto overflow-hidden"
        >
        <div class="relative w-full aspect-video overflow-hidden">
            <img
                src="@/assets/done-1.jpg"
                class="absolute inset-0 w-full h-full object-cover view"
            />
            <img
                ref="cup"
                src="@/assets/done-2.png"
                class="absolute bottom-[-6%] left-[0%] w-full opacity-0 element cup"
            />
        </div>

        <div v-if="!storyPlaying" class="step-container">
            <button
                @click="$emit('next')"
                class="mt-6 cursor-pointer"
              >
                <img src="@/assets/icon-reset.png" alt="Next"/>
            </button>
            <div class="description" ref="description">
                Grinding & Brewing - The roasted beans are ground and brewed into coffee.
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import * as anime from 'animejs';

export default {
  name: 'SlideSix',
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
      anime.set(this.$refs['cup'], { opacity: 0, translateX: -50 });
    },
    animate() {
      const tl = anime.createTimeline({ autoplay: true });
      tl.add('.view', {
        opacity: [0, 1],
        translateY: [50, 0],
        duration: 100,
      })
        .add(
          '.cup',
          {
            opacity: [0, 1],
            translateX: [50, 0],
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
