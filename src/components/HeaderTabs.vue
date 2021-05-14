<template>
  <div class="py-3 space-x-2 border-b border-gray-400 sm:space-x-4">
    <template v-for="tab in tabs" :key="tab.key">
      <button
        title="Preview"
        aria-label="Preview"
        class="
          px-2
          sm:px-3
          py-px
          sm:py-1
          text-xl
          rounded
          focus:outline-none
          transition-all
          duration-2000
          text-indigo-700
        "
        :class="activeVal === tab.key ? 'bg-indigo-100 font-semibold' : ''"
        @click="setTab(tab)"
      >
        {{ tab.title }}
      </button>
    </template>
  </div>
</template>
<script>
import { computed } from "vue";
export default {
  props: {
    tabs: Array,
    modelValue: String,
  },
  emits: ["update:modelValue"],
  setup(props, { emit }) {
    const activeVal = computed({
      get: () => props.modelValue,
      set: (val) => emit("update:modelValue", val),
    });
    function setTab(tab) {
      activeVal.value = tab.key;
    }
    return {
      activeVal,
      setTab,
    };
  },
};
</script>
