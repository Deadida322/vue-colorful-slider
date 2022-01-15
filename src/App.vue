<template>
  <div id="app">
    <ColorfulSlider :arrows="true" :wheelActive="false" :sidebar="true">
      <Slide>
        <h1>This is a first slide </h1>
      </Slide>
      <Slide>
        <h1>Yea, there is a second slide</h1>
      </Slide>
      <Slide>
        <h1>So many slides, as you want</h1>
      </Slide>
      <Slide>
        <h1>the last slide</h1>
      </Slide>
    </ColorfulSlider>
  </div>
</template>

<script>
import{ ColorfulSlider, Slide} from '@/colorfulslider.js'
export default {
  data() {
    return {
      captions: [
        'Our team',
        'About',
        'Contact us',
        'Our patreon'
      ]
    }
  },
  name: 'App',
  components: {
    ColorfulSlider,
    Slide
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #262626;
  margin-top: 0px;
  width: 98vw;
  height: 98vh;
}
h1{
  margin-top: 150px;
}
</style>
