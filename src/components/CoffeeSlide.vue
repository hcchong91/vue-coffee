<template>
  <div class="container mx-auto py-12">
    <h1 class="text-3xl font-bold mb-6 text-center">
      The Process of Making Coffee
    </h1>
    <div class="coffee-infographic">
      <swiper @swiper="onSwiper" @slide-change="onSlideChange" :slides-per-view="1" :space-between="50" :allow-touch-move="false" :effect="'fade'" :modules="modules" class="coffeeSwiper">
        <!-- Slide 1 -->
        <swiper-slide>
          <SlideOne :key="restartKey" @finished="onSlideOneFinished" @next="gotoNext" />
        </swiper-slide>

        <!-- Slide 2 -->
        <swiper-slide>
          <SlideTwo :key="restartKey" :active="currentSlide === 1" @next="gotoNext" />
        </swiper-slide>

        <!-- Slide 3 -->
        <swiper-slide>
          <SlideThree :key="restartKey" :active="currentSlide === 2" @next="gotoNext" />
        </swiper-slide>

        <!-- Slide 4 -->
        <swiper-slide>
          <SlideFour :key="restartKey" :active="currentSlide === 3" @next="gotoNext" />
        </swiper-slide>

        <!-- Slide 5 -->
        <swiper-slide>
          <SlideFive :key="restartKey" :active="currentSlide === 4" @next="gotoNext" />
        </swiper-slide>

        <!-- Slide 6 -->
        <swiper-slide>
          <SlideSix :key="restartKey" :active="currentSlide === 5" @next="restartSlide" />
        </swiper-slide>
      </swiper>
    </div>
  </div>
</template>

<script>
import { Swiper, SwiperSlide } from "swiper/vue"
import { EffectFade } from 'swiper/modules'
import "swiper/css"
import 'swiper/css/effect-fade'
import { createTimeline } from "animejs"
import SlideOne from "./SlideOne.vue"
import SlideTwo from "./SlideTwo.vue"
import SlideThree from "./SlideThree.vue"
import SlideFour from "./SlideFour.vue"
import SlideFive from "./SlideFive.vue"
import SlideSix from "./SlideSix.vue"

export default {
  components: {
    Swiper,
    SwiperSlide,
    SlideOne,
    SlideTwo,
    SlideThree,
    SlideFour,
    SlideFive,
    SlideSix,
  },

  data() {
    return {
      modules: [EffectFade],
      swiper: null,
      storyPlaying: false,
      currentSlide: 0,
      restartKey: 0,
      // parent tracks story state for slides 3+
    };
  },

  methods: {
    onSwiper(swiper) {
      this.swiper = swiper;
    },
    onSlideChange(swiper) {
      this.currentSlide = swiper.activeIndex;
    },
    onSlideOneFinished() {
      // optional hook when slide one animation ends
    },
    gotoNext() {
      // advance the swiper; components trigger this when they want the next slide
      if (this.swiper) this.swiper.slideNext();
    },
    nextSlide(slideIndex) {
      this.storyPlaying = true;
      // For remaining slides, just move to next after a delay or something
      setTimeout(() => {
        this.storyPlaying = false;
        if (this.swiper) this.swiper.slideNext();
      }, 1000); // simulate animation time
    },
    startStory() {
      this.storyPlaying = true;

      if (!this.storyPlayed) {
        const tl = createTimeline({
          autoplay: true,
          onComplete: () => {
            // after the animation finishes
            this.storyPlaying = false;
            this.storyPlayed = true;
            this.step1done = true;
          },
        });


        tl
          .add(".story", {
            opacity: [0, 1],
            duration: 200,
          })
          .add(
            ".farmer",
            {
              opacity: [0, 1],
              translateX: [50, 0],
              duration: 400,
            },
            "-=200",
          )
          .add(
            ".plant",
            {
              opacity: [0, 1],
              translateY: [50, 0],
              duration: 800,
            },
            "-=400",
          )
          .add(
            ".basket",
            {
              opacity: [0, 1],
              translateY: [30, 0],
              duration: 1200,
            },
            "-=600",
          );

        for (let i = 1; i <= 8; i++) {
          tl.add(
            `.plant-${i}`,
            {
              opacity: [0, 1],
              translateY: [30, 0],
              duration: 1600,
            },
            "-=800",
          );
        }
      } else {
        this.nextSlide(2);
      }
    },
    resetScene() {
      // not much to clean up in parent
    },
    restartSlide() {
      // reset all state and go back to first slide
      this.storyPlayed = false;
      this.step1done = false;
      this.storyPlaying = false;
      this.restartKey++;
      if (this.swiper) this.swiper.slideTo(0);
    },
  }
}
</script>

<style scoped>
.scene {
  position: relative;
  height: 500px;
  overflow: hidden;
}

.bg {
  width: 100%;
}

.farmer {
  position: absolute;
  opacity: 0;
}

.basket {
  position: absolute;
  opacity: 0;
}

.plant {
  position: absolute;
  opacity: 0;
}

.story {
  opacity: 0;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  cursor: pointer;
}
</style>
