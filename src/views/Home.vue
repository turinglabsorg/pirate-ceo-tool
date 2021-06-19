<template>
  <div class="home">
    <h1 style="margin-top:10vh">Be a Pirate CEO</h1>
    <TakeGif msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import TakeGif from '@/components/TakeGif.vue'

export default {
  name: 'Home',
  components: {
    TakeGif
  }
}
</script>
