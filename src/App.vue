<template>
  <CoffeeSlide/>
  <CoffeeContent/>
  <Coffee3D/>
</template>

<script>
import CoffeeSlide from './components/CoffeeSlide.vue'
import CoffeeContent from './components/CoffeeContent.vue'
import Coffee3D from './components/Coffee3D.vue'

export default {
  components:{
    CoffeeSlide,
    CoffeeContent,
    Coffee3D
  }
}
</script>