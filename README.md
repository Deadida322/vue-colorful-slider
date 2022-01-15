# vue-colorful-slider

An easy-to-use and quite customizable slider for Vue.js
Bright colors and unusual layout will make users of your application surprised

![Preview](https://github.com/Deadida322/vue-colorful-slider/blob/main/prevue.jpg?raw=true)
***
### install
    npm install vue-colorful-slider
### Features

* Easy to use
* Light and dark themese
* 5 ways to navigate
    * Touchpad/wheel
    * Dots
    * Keyboard Arrows
    * Slider Arrows
    * Sidebar Captions
* Breathtaking slide change 3d animation
* Support for autoplay

### Usage
``` html 
<template>
  <div id="app">
    <ColorfulSlider :sidebar="true">
      <Slide>
        This is a first slide 
      </Slide>
      <Slide>
        Yea, there is a second slide
      </Slide>
    </ColorfulSlider>
  </div>
</template>
```
```js
<script>
import {ColorfulSlider, Slide} from 'vue-colorful-slider'
export default {
  data() {
    return {
    }
  },
  name: 'App',
  components: {
    ColorfulSlider,
    Slide
  }
}
</script>
``` 
### Props

* dots - activate/deactivate dots navigation (__bool__) 
    ```js
    default: true
    ```
* arrows - activate/deactivate arrows navigation (__bool__)
    ```js
    default: false
    ```
* light - enabling a light theme (__bool__)
    ```js
    default: true
    ```
* autoplay - enabling slider autoplay
    ```js
    default: false
    ```
* autoplaySpeed - slide change interval, ms (__Number__)
    ```js
    default: 3000
    ```
* sidebar - activate/deactivate captions navigation (__bool__)
    ```js
    default: false
    ```
* captions - slide names for sidebar (__Array__)

>You must pass as many captions as slides, otherwise the missing slides will be marked with an ordinal number in the sidebar.

```js
...
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
...
```

* wheelActive - activate/deactivate touchpad or wheel navigation (__bool__)
```js
 default: true
```

[See source code](https://github.com/Deadida322/vue-colorful-slider)

[Ask a question](https://www.instagram.com/deadida32/)





