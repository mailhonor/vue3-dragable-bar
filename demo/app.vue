<template>
  <div>
    <div ref="ref1" class="dragWrap">
      <div class="leftBox" :style="{ width: leftRightMagic - 10 + 'px' }"></div>
      <div ref="ref2" class="rightBox" :style="{ left: leftRightMagic + 20 + 'px' }">
        <div class="topBox" :style="{ height: topBottomMagic - 10 + 'px' }"></div>
        <div class="bottomBox" :style="{ top: topBottomMagic + 20 + 'px' }">
        </div>
        <vue3DragableBar mode="horizontal" :top=topBottomMagic :height=10 @resize="dragResizeTopBottomOne"
          @resizeOver="dragResizeTopBottomOver">
          <div class="dragBarInner"></div>
        </vue3DragableBar>
      </div>
      <vue3DragableBar mode="vertical" :left=leftRightMagic :width=10 @resize="dragResizeLeftRightOne"
        @resizeOver="dragResizeLeftRightOver">
        <div class="dragBarInner"></div>
      </vue3DragableBar>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import vue3DragableBar from "vue3-dragable-bar"

const ref1 = ref()
const leftRightMagic = ref(300)
const dragResizeLeftRightOne = (offset: number) => {
  if (offset < 100) {
    return
  }
  if (offset > ref1.value.clientWidth - 100) {
    return
  }
  leftRightMagic.value = offset
}
const dragResizeLeftRightOver = (offset: number) => {
  if (offset < 100) {
    offset = 100
  }
  if (offset > ref1.value.clientWidth - 100) {
    offset = 100
  }
  leftRightMagic.value = offset
}

const ref2 = ref()
const topBottomMagic = ref(200)
const dragResizeTopBottomOne = (offset: number) => {
  if (offset < 50) {
    return
  }
  if (offset > ref2.value.clientHeight - 50) {
    return
  }
  topBottomMagic.value = offset
}
const dragResizeTopBottomOver = (offset: number) => {
  if (offset < 50) {
    offset = 50
  }
  if (offset > ref2.value.clientHeight - 50) {
    offset = 50
  }
  topBottomMagic.value = offset
}

onMounted(() => {
});
</script>

<style scoped>
.dragWrap {
  position: relative;
  width: 100%;
  height: 500px;
}

.leftBox {
  position: absolute;
  left: 0px;
  top: 0px;
  bottom: 0px;
  border: 1px solid rgb(140, 140, 140);
}

.rightBox {
  position: absolute;
  right: 0px;
  top: 0px;
  bottom: 0px;
  border: 1px solid rgb(140, 140, 140);
}

.dragBarInner {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: rgb(140, 140, 140);
}

.dragBarInner:hover {
  background-color: rgb(101, 101, 101);
}

.vue3DragableBar-moving .dragBarInner,
.vue3DragableBar-moving .dragBarInner:hover {
  background-color: green;
}

.topBox {
  position: absolute;
  left: 2px;
  right: 2px;
  top: 2px;
  border: 1px solid rgb(140, 140, 140);
}

.bottomBox {
  position: absolute;
  left: 2px;
  right: 2px;
  bottom: 2px;
  border: 1px solid rgb(140, 140, 140);
}
</style>