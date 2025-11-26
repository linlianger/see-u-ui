<template>
	<!-- #ifdef H5 -->
	<view :id="seeButtonId" class="see-button" :class="itemClass" @click="onTouchstart($event)">
	<!-- #endif -->
	<!-- #ifndef H5 -->
	<view :id="seeButtonId" class="see-button" :class="itemClass" @tap="onTap" @touchstart="onTouchstart($event)">
	<!-- #endif -->
		<!-- <slot></slot> -->
		<view class="test">
			asd
		</view>
		<view
			class="see-button-ripple"
			:class="{ 'active': active }"
			:style="{
				top: rippleTop + 'px',
				left: rippleLeft + 'px',
				width: field.finalWidth + 'px',
				height: field.finalWidth + 'px',
				'background-color': rippleColor
			}"
		></view>
	</view>
</template>

<script lang="ts" setup>
import { ref, nextTick, getCurrentInstance } from 'vue'
import type { TouchEvent, ClientRectData } from './type'

let globalId = 0
const instance = getCurrentInstance()

/** ---------- props ---------- */
const props = withDefaults(
  defineProps<{
	  rippleColor?: string
	  itemClass?: string
  }>(),
  {
    rippleColor: 'rgba(0, 0, 0, .15)',
    itemClass: ''
  }
)

/** ---------- emits ---------- */
const emit = defineEmits<{
  (e: 'onTap'): void
}>()

/** ---------- state ---------- */
globalId++
const rippleTop = ref(0)
const rippleLeft = ref(0)
const active = ref(false)
const field = ref<any>({})
const seeButtonId = 'seeButton_' + globalId

/** ---------- methods ---------- */
const onTap = () => emit('onTap')

const onTouchstart = (e: any) => {
  active.value = false
  nextTick(() => activeWaves(e))
}

const activeWaves = (e: TouchEvent) => {
  getClientRect().then((data: ClientRectData) => {
    if (!data?.height) return

    data.finalWidth = data.height > data.width ? data.height : data.width
    if (!data.finalWidth) return

    field.value = data

    let touchesX: number
    let touchesY: number

    // #ifdef MP-BAIDU
    touchesX = e.changedTouches[0].clientX
    touchesY = e.changedTouches[0].clientY
    // #endif

    // #ifdef MP-ALIPAY
    touchesX = e.detail.clientX
    touchesY = e.detail.clientY
    // #endif

    // #ifndef MP-BAIDU || MP-ALIPAY
    touchesX = e.touches[0].clientX
    touchesY = e.touches[0].clientY
    // #endif

    rippleTop.value = touchesY - data.top - data.finalWidth / 2
    rippleLeft.value = touchesX - data.left - data.finalWidth / 2

    nextTick(() => active.value = true)
  })
}

const getClientRect = () => {
  return new Promise(resolve => {
    const query = uni.createSelectorQuery().in(instance)
    const id = '#' + seeButtonId

    query
      .select(id)
      .boundingClientRect(data => {
        resolve(data)
      })
      .exec()
  })
}
</script>

<style lang="scss" scoped>
.see-button {
	position: relative;
	overflow: hidden;
	
	.see-button-ripple {
		position: absolute;
		border-radius: 100%;
		background-clip: padding-box;
		pointer-events: none;
		user-select: none;
		transform: scale(0);
		opacity: 1;
	}
	
	.active {
		opacity: 0;
		transform: scale(2);
		transition: opacity 1.2s ease-out, transform 0.5s ease-out;
	}
}


</style>
