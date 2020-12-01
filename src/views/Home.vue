<template>
  <div class="home">
    <Timer/>
  </div>
</template>

<script>
// @ is an alias to /src
import Timer from '@/components/Timer.vue'

export default {
  name: 'Home',
  components:{
    Timer
  },
}
</script>
<style scoped>
  .home{
    justify-content: center;
    align-items: center;
    display: flex;
    height: 100%;
  }
</style>
