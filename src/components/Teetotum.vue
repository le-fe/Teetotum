<template>
  <div class="py-4">
    <div class="flex flex-wrap">
      <div class="p-12 md:w-1/2 flex flex-col items-start">
        <span>Prize number: {{ prizeNumber }}</span>
        <button
          type="button"
          @click="!rolling && prizeNumber < 8 && prizeNumber++"
          :disabled="rolling || prizeNumber === 8"
        >
          Add
        </button>
        <button
          type="button"
          @click="!rolling && prizeNumber > 2 && prizeNumber--"
          :disabled="rolling || prizeNumber === 2"
        >
          Remove
        </button>
      </div>

      <div class="p-12 md:w-1/2 flex flex-col items-start">
        <div class="wheel-wrapper">
          <div class="wheel-pointer" @click="onClickRotate">Start</div>
          <div
            class="wheel-bg bg-indigo-300"
            :class="{ freeze: freeze }"
            :style="`transform: rotate(${wheelDeg}deg)`"
          >
            <div class="prize-list">
              <div
                class="prize-item-wrapper"
                v-for="(item, index) in prizeList"
                :key="index"
              >
                <div
                  class="prize-item"
                  :style="`transform: rotate(${
                    (360 / prizeList.length) * index
                  }deg)`"
                >
                  <div class="prize-name">
                    {{ item.name }}
                  </div>
                  <div class="prize-icon">
                    <img class="rounded-lg" :src="item.icon" />
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { ref, computed, watch } from "vue";
export default {
  setup(props) {
    const freeze = ref(false);
    const rolling = ref(false);
    const wheelDeg = ref(0);
    const prizeNumber = ref(8);
    const prizeListOrigin = ref([
      {
        icon: "https://picsum.photos/40?random=1",
        name: "$10000",
      },
      {
        icon: "https://picsum.photos/40?random=6",
        name: "Thank you!",
      },
      {
        icon: "https://picsum.photos/40?random=2",
        name: "$500",
      },
      {
        icon: "https://picsum.photos/40?random=3",
        name: "$100",
      },
      {
        icon: "https://picsum.photos/40?random=6",
        name: "Thank you!",
      },
      {
        icon: "https://picsum.photos/40?random=4",
        name: "$50",
      },
      {
        icon: "https://picsum.photos/40?random=5",
        name: "$10",
      },
      {
        icon: "https://picsum.photos/40?random=6",
        name: "Thank you!",
      },
    ]);
    const prizeList = computed(() =>
      prizeListOrigin.value.slice(0, prizeNumber.value)
    );
    function onClickRotate() {
      if (rolling.value) return;
      const result = Math.floor(Math.random() * prizeList.value.length);
      roll(result);
    }
    function roll(result) {
      rolling.value = true;
      wheelDeg.value =
        wheelDeg.value -
        (wheelDeg.value % 360) +
        6 * 360 +
        (360 - (360 / prizeList.value.length) * result);
      setTimeout(() => {
        rolling.value = false;
        alert("Result：" + prizeList.value[result].name);
      }, 4500);
    }
    watch(prizeNumber, (prizeNumber, prevPrizeNumber) => {
      freeze.value = true;
      wheelDeg.value = 0;

      setTimeout(() => {
        freeze.value = false;
      }, 0);
    });
    return {
      freeze,
      rolling,
      wheelDeg,
      prizeNumber,
      prizeListOrigin,
      prizeList,
      // Methods
      onClickRotate,
      roll,
    };
  },
};
</script>
<style>
.wheel-wrapper {
  width: 300px;
  height: 300px;
  position: relative;
}
.wheel-pointer {
  width: 60px;
  height: 60px;
  border-radius: 1000px;
  background: yellow;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  line-height: 60px;
  z-index: 10;
  cursor: pointer;
}
.wheel-pointer::after {
  content: "";
  position: absolute;
  top: -32px;
  left: 50%;
  border-width: 0 8px 40px;
  border-style: solid;
  border-color: transparent transparent yellow;
  transform: translateX(-50%);
}
.wheel-bg {
  width: 100%;
  height: 100%;
  border-radius: 1000px;
  overflow: hidden;
  transition: transform 4s ease-in-out;
}
.wheel-bg.freeze {
  transition: none;
  background: red;
}
.prize-list {
  width: 100%;
  height: 100%;
  position: relative;
  text-align: center;
}
.prize-item-wrapper {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 150px;
  height: 150px;
}
.prize-item {
  width: 100%;
  height: 100%;
  transform-origin: bottom;
}
.prize-item .prize-name {
  padding: 16px 0;
}
</style>
