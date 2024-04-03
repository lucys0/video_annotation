<template>
  <div style="max-width: 800px; margin: 0 auto" class="q-gutter-md">
    <!-- <h5>Tutorial Videos</h5>
    <div class="text-body1">
      <q-video
        :ratio="16 / 9"
        src=""
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
  {
    keys: ['left-arrow', 'right-arrow'],
    description: 'previous / next frame'
  },
  {
    keys: ['enter'],
    description: 'add a new video segment'
  },
  {
    keys: ['command + s', 'ctrl + s'],
    description: 'save annotations'
  },
]
</script>
