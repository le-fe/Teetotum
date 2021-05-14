<template>
  <div class="bg-indigo-100 h-screen px-10 py-10 flex flex-col">
    <div
      class="
        w-full
        h-full
        bg-white
        px-8
        rounded-lg
        border border-gray-200
        flex-1
        overflow-hidden
      "
    >
      <HeaderTabs :tabs="tabs" v-model="activeTab" />
      <template v-if="activeComponent">
        <component :is="activeComponent" />
      </template>
    </div>
    <Footer />
  </div>
</template>

<script setup>
import Footer from "./components/Footer.vue";
import HeaderTabs from "./components/HeaderTabs.vue";
import Teetotum from "./components/Teetotum.vue";
import Dice from "./components/Dice.vue";
import { ref, computed } from "vue";
const activeTab = ref("teetotum");
const activeComponent = computed(
  () => tabs.find((x) => x.key === activeTab.value)?.component
);
const tabs = [
  { key: "teetotum", title: "Teetotum 🦄", component: Teetotum },
  { key: "dice", title: "Dice 😅", component: Dice },
];
</script>
