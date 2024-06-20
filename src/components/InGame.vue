<script setup>
import { ref, onMounted } from 'vue'

const height = ref()

// outerのref参照用変数
const outer = ref(null)

let stepList = ref()

let character = ref({})

onMounted(() => {
  // outerのrefを参照
  outer.value.focus()
  const outerElem = outer.value
  console.log('outerElem :>> ', outerElem);
  const outerRect = outerElem.getBoundingClientRect()
  console.log('outerRect.height :>> ', outerRect.height);

  // const cellSize = ref(outerRect.height / 10)
  const cellSize = ref(outerElem.clientHeight / 10)

  console.log('cellSize.value :>> ', cellSize.value);

  // 各段差の高さ
  const stepLevelList = [1,2,3,1,3,4,5,5,5,4]
  stepList.value = stepLevelList.map((stepLevel, index) => {
    const width = cellSize.value
    const height = stepLevel * cellSize.value
    const top = outerElem.clientHeight - height
    const left = cellSize.value * index
    return {
      width,
      height,
      top,
      left
    }
  })
  console.log('stepList.value :>> ', stepList.value);


  // キャラクターの位置設定
  const characterWidth = cellSize.value / 2
  const characterHeight = cellSize.value / 2
  const characterTop = outerElem.clientHeight - stepList.value[0].height - characterHeight
  const characterLeft = stepList.value[1].left / 2
  character.value = {
    width: characterWidth,
    height: characterHeight,
    top: characterTop,
    left: characterLeft
  }
  console.log('character.value :>> ', character.value);
})

const jump = () => {
  console.log('jump :>> ');
}
</script>

<template>
  <div ref="outer" class="game_outer" @click="jump()">
    <!-- キャラクター -->
    <div
      v-if="Object.keys(character).length > 0"
      class="character"
      :style="`
        width: ${character.width}px;
        height: ${character.height}px;
        top: ${character.top}px;
        left: ${character.left}px;
      `"></div>

    <!-- 段差 -->
    <div
      v-for="(step, index) in stepList"
      :key="`step_${index}`"
      class="step"
      :style="`
        width: ${step.width}px;
        height: ${step.height}px;
        top: ${step.top}px;
        left: ${step.left}px;
      `">
    </div>
  </div>
</template>

<style scoped>
.game_outer {
  width: 80svw;
  height: 80svw;
  border: 1svw solid black;
  position: relative;
  box-sizing:content-box;
}

.character {
  position: absolute;
  background: mediumaquamarine;
}
.step {
  display: flex;
  position: absolute;
  background: gainsboro;
}
</style>
