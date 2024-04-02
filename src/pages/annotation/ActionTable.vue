<template>
  <q-table dense flat :rows="reversedActionList" row-key="start" :columns="columnList" :pagination="{ rowsPerPage: 0 }"
    :filter="actionFilterList" :filter-method="actionFilter" :sort-method="actionSort" binary-state-sort>
    <template v-slot:no-data>
      <div class="text-center q-pa-md">
        <span>&#9888;</span> No data
      </div>
    </template>
    <template v-slot:top="props">
      <div class="col-6 q-table__title">Video Segments</div>
      <q-space></q-space>
      <q-btn-group flat>
        <q-btn size="sm" outline icon="new_label" label="add & advance" @click="handleAddAdvance">
          <q-tooltip>add current range and advance for next</q-tooltip>
        </q-btn>
        <q-btn size="sm" outline icon="add" label="add" @click="handleAdd">
          <q-tooltip>add current range (+)</q-tooltip>
        </q-btn>
        <q-btn size="sm" outline icon="arrow_forward" label="advance" @click="handleAdvance">
          <q-tooltip>advance current range</q-tooltip>
        </q-btn>
        <q-btn size="sm" outline icon="clear_all" label="clear" @click="handleClearAll">
          <q-tooltip>Bulk clear all actions</q-tooltip>
        </q-btn>
      </q-btn-group>
    </template>
    <template v-slot:body="props">
      <q-tr :class="{
          'bg-warning': props.row.end - props.row.start <= 0,
          'bg-green-2': props.row === annotationStore.currentThumbnailAction && !q.dark.isActive,
          'bg-green-8': props.row === annotationStore.currentThumbnailAction && q.dark.isActive
        }">
        <q-tooltip anchor="top middle" v-if="props.row.end - props.row.start <= 0">Duration should be greater than 0.
        </q-tooltip>
        <q-td auto-width>
          <q-input style="min-width: 100px" v-model.number="props.row.start" dense borderless type="number"
            :debounce="1500" @mousewheel.prevent></q-input>
        </q-td>
        <q-td auto-width>
          <q-input style="min-width: 100px" v-model.number="props.row.end" dense borderless type="number"
            :debounce="1500" @mousewheel.prevent></q-input>
        </q-td>
        <q-td auto-width>
          <!-- {{ utils.toFixed2(props.row.end - props.row.start) }} -->
          {{ props.row.end - props.row.start }}
        </q-td>
        <q-td>
          <q-input v-model="props.row.description" dense borderless type="text" class="description-input"
            :key="'desc-input-' + props.row.start"></q-input>
        </q-td>
        <q-td auto-width>
          <q-btn-group spread flat>
            <q-btn flat dense icon="gps_fixed" style="width: 100%" @click="handleGoto(props.row)">
              <q-tooltip>Locate to the skill</q-tooltip>
            </q-btn>
            <q-btn flat dense icon="edit_location_alt" style="width: 100%" @click="handleSet(props.row)">
              <q-tooltip>Set current left / right frame as this skill's start / end</q-tooltip>
            </q-btn>
            <q-btn flat dense icon="delete" color="negative" style="width: 100%"
              @click="handleDelete(props.row)"></q-btn>
            <q-btn flat dense icon="cancel" :color="props.row.is_mistake ? 'grey' : 'warning'" style="width: 100%"
              @click="handleSetMistake(props.row)">
              <q-tooltip>
                {{ props.row.is_mistake ? 'Set this segment as NOT a mistake' : 'Set this segment as a mistake' }}
              </q-tooltip>
            </q-btn>
          </q-btn-group>
        </q-td>
      </q-tr>
    </template>
  </q-table>
  <ActionThumbnailPreview />
</template>

<script setup>
import { useQuasar } from 'quasar'
import { computed, onMounted, onUnmounted, ref } from 'vue'

import ActionThumbnailPreview from '~/components/ActionThumbnailPreview.vue'
import { ActionAnnotation } from '~/libs/annotationlib.js'
import utils from '~/libs/utils.js'
import { useAnnotationStore } from '~/store/annotation.js'
import { useConfigurationStore } from '~/store/configuration.js'
import { useMainStore } from '~/store/index.js'
import { nextTick } from 'vue';

const annotationStore = useAnnotationStore()
const configurationStore = useConfigurationStore()
const mainStore = useMainStore()
const q = useQuasar()

const reversedActionList = computed(() => {
  // Creates a shallow copy of the array and reverses it
  return [...annotationStore.actionAnnotationList].reverse();
});

const columnList = [
  {
    name: 'start',
    align: 'center',
    label: 'start frame',
    field: 'start',
    sortable: true,
    sort: (a, b, rowA, rowB) => (a !== b ? a - b : rowA.end - rowB.end)
  },
  {
    name: 'end',
    align: 'center',
    label: 'end frame',
    field: 'end',
    sortable: true,
    sort: (a, b, rowA, rowB) => (a !== b ? a - b : rowA.start - rowB.start)
  },
  {
    name: 'duration',
    align: 'center',
    label: 'duration'
  },
  {
    name: 'description',
    align: 'center',
    label: 'skill description',
    field: 'description'
  },
  {
    name: 'operation',
    align: 'center',
    label: 'operation',
    field: 'operation'
  }
]

// header
const handleAdd = () => {
  annotationStore.actionAnnotationList.push(
    new ActionAnnotation(
      annotationStore.leftCurrentFrame,
      annotationStore.rightCurrentFrame,
      configurationStore.actionLabelData[0].id,
      configurationStore.actionLabelData[0].objects[0],
      configurationStore.actionLabelData[0].color,
      ''
    )
  )
  // automatically move to the focus to the description field
  nextTick(() => {
    const inputs = document.querySelectorAll('.description-input');
    if (inputs.length) {
      inputs[0].focus();
    }
  });
}

// a function for advance
const handleAdvance = () => {
  annotationStore.leftCurrentFrame = annotationStore.rightCurrentFrame + 1
  annotationStore.rightCurrentFrame = annotationStore.rightCurrentFrame + 1
}

const handleAddAdvance = () => {
  handleAdd()
  handleAdvance()
}
const handleClearAll = () => {
  if (annotationStore.actionAnnotationList.length !== 0) {
    utils.confirm('Are you sure to delete ALL actions?').onOk(() => {
      annotationStore.currentThumbnailAction = null
      annotationStore.actionAnnotationList = []
    })
  } else {
    utils.notify('There are no actions!', 'warning')
  }
}

/// filter
const actionFilterList = ref({})
configurationStore.actionLabelData.forEach((label) => {
  actionFilterList.value[label.id] = true
})

const actionFilter = (rows, filter) => {
  return rows.filter((row) => filter[row.action])
}

// sort
annotationStore.currentSortedActionList = annotationStore.actionAnnotationList
const actionSort = (rows, sortBy, descending) => {
  const sortedRows = rows.slice().sort((a, b) => {
    const sortVal = a[sortBy] < b[sortBy] ? -1 : 1
    return descending ? sortVal : -sortVal
  })
  annotationStore.currentSortedActionList = sortedRows
  return sortedRows
}

// body

const objectOptionMap = ref({})
for (let action of configurationStore.actionLabelData) {
  const objectOptionList = []
  for (let object of configurationStore.objectLabelData) {
    if (action.objects.includes(object.id)) {
      objectOptionList.push({
        label: object.name,
        value: object.id
      })
    }
  }
  objectOptionMap.value[action.id] = objectOptionList
}

/// operation
const handleGoto = (row) => {
  if (typeof row.start === 'number') {
    // annotationStore.leftCurrentFrame = utils.time2index(row.start)
    annotationStore.leftCurrentFrame = row.start
  }
  if (typeof row.end === 'number') {
    // annotationStore.rightCurrentFrame = utils.time2index(row.end)
    annotationStore.rightCurrentFrame = row.end
  }
}
const handleSet = (row) => {
  // row.start = utils.index2time(annotationStore.leftCurrentFrame)
  // row.end = utils.index2time(annotationStore.rightCurrentFrame)
  row.start = annotationStore.leftCurrentFrame
  row.end = annotationStore.rightCurrentFrame
  handleAdvance()
}
const handleDelete = (row) => {
  utils.confirm('Are you sure to delete this segment?').onOk(() => {
    annotationStore.currentThumbnailAction = null
    for (let i in annotationStore.actionAnnotationList) {
      if (annotationStore.actionAnnotationList[i] === row) {
        annotationStore.actionAnnotationList.splice(i, 1)
      }
    }
  })
}
const handleSetMistake = (row) => {
  // if not a mistake, set as a mistake
  if (!row.is_mistake) {
    utils.confirm('Are you sure to set this segment as a mistake?').onOk(() => {
      row.is_mistake = true
    })
  }
  // if a mistake, set as not a mistake
  else {
    utils.confirm('Are you sure to set this segment as NOT a mistake?').onOk(() => {
      row.is_mistake = false
    })
  }
}

// keybindings
const handleKeyup = (event) => {
  event.stopPropagation()
  if (event.target.nodeName.toLowerCase() === 'input') {
    // return false
    // exit the input mode if the key is enter
    if (event.code === 'Enter') {
      event.target.blur()
    } 
    return false
  }
  if (event.code === 'Equal') {
    event.preventDefault()
    handleAdd()
  } else if (event.code === 'Enter') {
    event.preventDefault()
    handleAddAdvance()
  }
}
onMounted(() => {
  window.addEventListener('keyup', handleKeyup, true)
})
onUnmounted(() => {
  window.removeEventListener('keyup', handleKeyup, true)
})
</script>
