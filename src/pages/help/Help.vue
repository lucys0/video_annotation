<template>
  <div
    style="max-width: 800px; margin: 0 auto"
    class="q-gutter-md"
  >
    <!-- <h5>Tutorial Videos</h5>
    <div class="text-body1">
      <q-video
        :ratio="16 / 9"
        src="https://www.youtube.com/embed/videoseries?list=PLD-7XrNHCcFLv938DO4yYcTrgaff9BJjN"
      />
    </div> -->
    <h5>Instruction</h5>
    <div class="text-body1">
      Click the top-left menu button (&#x2630;) to load videos and save annotations. 
      [TODO]
    </div>
    <h5>Keyboard Shortcuts</h5>
    <div class="text-body1">
      <div
        class="row q-ma-none"
        v-for="shortcut in shortcuts"
      >
        <div
          class="col"
          style="line-height: 30px"
        >
          {{ shortcut.description }}
        </div>
        <div class="col text-center">
          <q-chip
            class="text-black"
            color="secondary"
            size="md"
            v-for="key in shortcut.keys"
            >{{ key.toUpperCase() }}
          </q-chip>
        </div>
      </div>
    </div>
    <!-- <h5>Touch Support</h5>
    <div class="text-body1">
      Mode modifier switches will appear for devices with touchscreens (e.g., tablets), replacing modifier keys (shift,
      alt, etc.).
    </div> -->
  </div>
</template>

<script setup>
const shortcuts = [
  {
    keys: ['space'],
    description: 'play / pause video segment'
  },
  // {
  //   keys: ['comma, <', 'period, >'],
  //   description: 'advance to previous / next keyframe'
  // },
  {
    keys: ['left-arrow', 'right-arrow'],
    description: 'previous / next frame'
  },
  // {
  //   keys: ['up-arrow', 'down-arrow'],
  //   description: 'cycle through the frame slider: right > range > left'
  // },
  // {
  //   keys: ['page-up', 'page-down'],
  //   description: 'jump to previous / next 10% of video'
  // },
  {
    keys: ['enter'],
    description: 'add a new video segment & advance, and automatically pause the video. If pressed again, save the current segement and continue playing the video.'
  },
  // {
  //   keys: ['delete'],
  //   description: 'delete currently active object'
  // },
  // {
  //   keys: ['shift'],
  //   description: 'duplicate currently active object when mouse down'
  // },
  // {
  //   keys: ['alt'],
  //   description: 'add new point to a region when mouse down'
  // },
  // {
  //   keys: ['backspace'],
  //   description: 'delete current point in a region when mouse down'
  // },
  // {
  //   keys: ['tab'],
  //   description: 'move to next field when editing objects'
  // }
]
</script>
