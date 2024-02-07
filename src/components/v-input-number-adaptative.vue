
<template>
  <div class="flex p-3">
    <img class="w-20 h-20 rounded-full p-1 object-cover border border-theme-main" :src="props.pictureURL">
    <div class="flex flex-col justify-center ml-3">
      <div v-if="props.label" class="text-sm font-semibold text-theme-main uppercase mb-1">
        {{props.label}}
      </div>
      <div class="relative flex items-center">
        <input type="text"
               class="min-w-[72px] p-2 rounded-md border-0 outline-0 opacity-50 ring-1 ring-inset ring-theme-main ring-opacity-40 focus:opacity-100"
               :class="[state.value ? 'ring-opacity-100' : '']"
               :style="GetFormattedStyle"
               :value="GetFormattedValue"
               @input="OnInput"
        />
        <span class="text-gray-700 whitespace-nowrap ml-3">
          {{props.description}}
        </span>
        <div class="absolute top-8 p-2 whitespace-nowrap pointer-events-none opacity-0"
             ref="valueDummy">{{GetFormattedValue}}</div>
      </div>
    </div>
  </div>
</template>

<script setup>

import { reactive, watch, computed, ref, onMounted, onUpdated } from 'vue'

const props = defineProps({
  pictureURL: {
    type: String,
    default: "https://storage.googleapis.com/pod_public/1300/150708.jpg"
  },
  label: {
    type: String,
    default: "Samuel is"
  },
  initialValue: {
    type: Number,
    default: undefined
  },
  description: {
    type: String,
    default: "hours old"
  },
})

const state = reactive({
  value: props.initialValue,
  width_px: 72
})
const valueDummy = ref(null);

const GetFormattedValue = computed(() => {
  // Value formatting only for display
  return state.value?.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
})

const GetFormattedStyle = computed(() => {
  return { width: state.width_px + 'px' }
})

const OnInput = async (e) => {
  // Soft reset of the input value
  state.value = undefined;
  // Updating input value (only digits are allowed to be added)
  state.value = e.target.value.replace(/\D/g, '');
}

onUpdated(() => {
  // When the dummy value innerText is applied, the input width is updated
  state.width_px = valueDummy.value.offsetWidth;
})

</script>
