<template>
  <div ref="selfRef" v-if="props.mode === 'vertical'" class="bar bar-x"
    :class="isMoving ? 'vue3-dragable-bar-moving' : ''"
    :style="{ left: offsetLeft - 1 + 'px', width: props.width + 'px' }">
    <slot></slot>
  </div>
  <div ref="selfRef" v-if="props.mode === 'horizontal'" class="bar bar-y"
    :class="isMoving ? 'vue3-dragable-bar-moving' : ''"
    :style="{ top: offsetTop - 1 + 'px', height: props.height + 'px' }">
    <slot></slot>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref, watch } from "vue"

let tmpWindow: any = window;
if (tmpWindow._vue3_dragable_bar_moving_ === undefined) {
  tmpWindow._vue3_dragable_bar_moving_ = false;
}

const props = defineProps({
  mode: {
    type: String,
    default: ''
  },
  top: {
    type: Number,
    default: 0
  },
  left: {
    type: Number,
    default: 0
  },
  width: {
    type: Number,
    default: 0
  },
  height: {
    type: Number,
    default: 0
  },
});

const isMoving = ref(false);

const emit = defineEmits(["resize", "resizeOver"]);

const selfRef = ref();
let is_vertical = true;
const offsetTop = ref(0);
const offsetLeft = ref(0);
let layerTop: number = 0;
let layerLeft: number = 0;
let layerWidth: number = 0;
let layerHeight: number = 0;
let relativeTop: number = 0;
let relativeLeft: number = 0;

const get_up_layer_position = () => {
  layerTop = 0;
  layerLeft = 0;
  layerWidth = 0;
  layerHeight = 0;
  let sd = selfRef.value;
  if (!sd) {
    return;
  }
  sd = sd.parentElement;
  if (!sd) {
    return;
  }
  let info = sd.getBoundingClientRect();
  layerTop = info.top;
  layerLeft = info.left;
  layerWidth = info.width;
  layerHeight = info.height;
}

function resize_fire(action: any, ev: MouseEvent) {
  let offset = 0;
  if (is_vertical) {
    offset = ev.clientX - layerLeft - relativeLeft;
    if (offset > layerWidth) {
      offset = layerWidth
    }
  }
  else {
    offset = ev.clientY - layerTop - relativeTop;
    if (offset > layerHeight) {
      offset = layerHeight;
    }
  }
  if (offset < 0) {
    offset = 0
  }
  if (action == "resizeOver") {
    if (!Object.keys(emit).includes("resizeOver")) {
      action = "resize"
    }
  }
  emit(action, offset);
}

function mousemove_handler(ev: MouseEvent) {
  if (!tmpWindow._vue3_dragable_bar_moving_) {
    return;
  }
  isMoving.value = true;
  ev.preventDefault();
  ev.stopPropagation();
  resize_fire("resize", ev);
}

function mouseup_handler(ev: MouseEvent) {
  isMoving.value = false;
  if (!tmpWindow._vue3_dragable_bar_moving_) {
    return;
  }
  tmpWindow._vue3_dragable_bar_moving_ = false;
  ev.preventDefault();
  ev.stopPropagation();
  resize_fire("resizeOver", ev);
  remove_handler();
}

function remove_handler() {
  document.body.setAttribute("vue3-dragable-bar", "")
  try {
    document.removeEventListener("mousemove", mousemove_handler, false);
  } catch { }
  try {
    document.removeEventListener("mouseup", mouseup_handler, false);
  } catch { }
}

const handleMouseDown = (ev: MouseEvent) => {
  remove_handler();
  document.body.setAttribute("vue3-dragable-bar", "moving")
  tmpWindow._vue3_dragable_bar_moving_ = true;
  get_up_layer_position();
  relativeTop = ev.offsetY;
  relativeLeft = ev.offsetX;
  document.addEventListener("mousemove", mousemove_handler, false);
  document.addEventListener("mouseup", mouseup_handler, false);
  isMoving.value = true;
}

onMounted(() => {
  is_vertical = (props.mode == "vertical");
  if (is_vertical) {
    offsetLeft.value = props.left;
  } else {
    offsetTop.value = props.top;
  }
  try {
    selfRef.value.addEventListener("mousedown", handleMouseDown);
  } catch { }
  watch([() => props.top, () => props.left], () => {
    if (is_vertical) {
      offsetLeft.value = props.left;
    } else {
      offsetTop.value = props.top;
    }
  });
});
</script>

<style scoped>
.bar {
  position: absolute;
  margin: 0px;
  padding: 0px;
}

.bar-x {
  top: 0px;
  bottom: 0px;
}

.bar-x:hover {
  cursor: col-resize;
}

.bar-y {
  left: 0px;
  right: 0px;
  line-height: 100%;
}

.bar-y:hover {
  cursor: row-resize;
}
</style>
