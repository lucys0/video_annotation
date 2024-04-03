<template>
  <div style="max-width: 800px; margin: 0 auto" class="q-gutter-md">
    <!-- <h5>Tutorial Videos</h5>
    <div class="text-body1">
      <q-video
        :ratio="16 / 9"
        src="https://www.youtube.com/embed/videoseries?list=PLD-7XrNHCcFLv938DO4yYcTrgaff9BJjN"
      />
    </div> -->
    <h5>Instruction</h5>
    <div class="text-body1">
      - Click Open Video (or press <q-chip color="secondary" size="md">Enter</q-chip>) to load a video.
      <br>
      - Click play/pause (or press <q-chip color="secondary" size="md">Space</q-chip>) to start/pause the video.
      <br>
      - Press <q-chip color="secondary" size="md">Enter</q-chip> to mark the end of a skill segment.
      <br>
      - Write the skill description in the text field at the bottom of the page.
      <br>
      - Press <q-chip color="secondary" size="md">Enter</q-chip> to save the segment and resume the video.
      <br>
      - To adjust any of the skill segments, click the segment (marked
      in blue) or the locate button on the corresponding row,
      and adjust the start and end times. Then press the set button on the corresponding row to save the changes. Press
      advance to move on.
      <br>
      - If the segment is a mistake, click the mistake button on the corresponding row.
      <br>
      - After labeling all the skill segments, click save or press <q-chip color="secondary" size="md">Command +
        S</q-chip> to save annotations.


    </div>
    <h5>Keyboard Shortcuts</h5>
    <div class="text-body1">
      <div class="row q-ma-none" v-for="shortcut in shortcuts">
        <div class="col" style="line-height: 30px">
          {{ shortcut.description }}
        </div>
        <div class="col text-center">
          <q-chip class="text-black" color="secondary" size="md" v-for="key in shortcut.keys">{{ key.toUpperCase() }}
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
    keys: ['enter'],
    description: 'open video'
  },
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
    description: 'add a new video segment'
    // , and automatically pause the video. If pressed again, save the current segement and continue playing the video.
  },
  // command/control + s for save
  {
    keys: ['command + s', 'ctrl + s'],
    description: 'save annotations'
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
