<template>
  <div class="w-full max-w-4xl mx-auto overflow-hidden position-relative">
    <div class="relative w-full aspect-video">
      <img
        src="@/assets/planting-1.jpg"
        class="absolute inset-0 w-full h-full object-cover view"
      />
      
      <img
        ref="plant"
        src="@/assets/planting-2.png"
        class="absolute bottom-[0%] left-[0%] w-full opacity-0 element plants"
      />
      <img
        ref="farmer"
        src="@/assets/planting-3.png"
        class="absolute bottom-[10%] left-[0%] w-full opacity-0 element farmer"
      />
    </div>

    <div v-if="!storyPlaying" class="step-container">
      <button
        @click="startStory"
        class="mt-6 cursor-pointer"
      >
        <img v-if="storyPlayed" src="@/assets/icon-next.png" alt="Next"/>
        <img v-else src="@/assets/icon-start.png" alt="Play"/>
      </button>
      <div class="description" ref="description">
        Planting - Coffee farming begins with planting young coffee trees in fertile
        soil.
      </div>
    </div>
  </div>
</template>

<script>
import { createTimeline } from 'animejs';

export default {
  name: 'SlideOne',
  data() {
    return {
      storyPlayed: false,
      storyPlaying: false,
    };
  },
  methods: {
    startStory() {
      this.storyPlaying = true;

      if (!this.storyPlayed) {
        const tl = createTimeline({
          autoplay: true,
          onComplete: () => {
            this.storyPlaying = false
            this.storyPlayed = true
            this.$emit('finished')
          },
        });

        tl
          .add('.view', {
            opacity: [0, 1],
            duration: 100,
          })
          .add(
            '.farmer',
            {
              opacity: [0, 1],
              translateY: [0, 50],
              duration: 200,
            },
            '-=100',
          )
          .add(
            '.plants',
            {
              opacity: [0, 1],
              translateY: [50, 0],
              duration: 400,
            },
            '-=200',
          )
      } else {
        this.$emit('next');
      }
    },
  },
};
</script>

<style scoped>
.element {
  opacity: 0;
}

/* show background view immediately */
.view {
  opacity: 1;
}

.story {
  opacity: 0;
}
</style>