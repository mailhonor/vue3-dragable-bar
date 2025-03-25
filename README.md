# vue3-dragable-bar

## 说明

网址: https://github.com/mailhonor/vue3-dragable-bar

在一个容器里, 拖动一个小div, 然后根据拖动后的位置, 做到resize的效果


最好见例子:

https://github.com/mailhonor/vue3-dragable-bar/blob/master/demo/app.vue

## 演示

http://linuxmail.cn/web/dragable-bar-demo.html

## 用法

```
import vue3DragableBar from "vue3-dragable-bar";

```

## props
```
const props = defineProps({
  mode: {
    type: String, // vertical 或 horizontal
    default: ''
  },
  top: {
    type: Number, // position:top
    default: 0
  },
  left: { // position:left
    type: Number,
    default: 0
  },
  width: { // 宽度
    type: Number,
    default: 0
  },
  height: { // 高度
    type: Number,
    default: 0
  },
});
```


## emits
```

// resize(offset), 拖动这个bar时的emit, offset 是 top/left
// resizeOver(offset), 拖动结束后的emit
const emit = defineEmits < {
  (e: 'resize', offset: number): void
  (e: 'resizeOver', offset: number): void
}> ()
```