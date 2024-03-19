<template>
    <img ref="imageRef" :style="widgetStyles" class="relative" draggable="true" @dragstart="dragStarted"
            src="https://media.smartbloks.ai/media/__dld?__ct=image/png&__mxd=f75319bd-5f43-47ab-aa30-5600bbdcd87a/media/7a8d23ee-2d6a-4b79-bc28-0d24de3aba8b" />
</template>

<script lang='ts' setup>
import { computed, onMounted, ref } from 'vue'

type Props = {
    parentBoundaries: {
        left: number
        right: number
        top: number
        bottom: number
    }
}

const imageRef = ref()

const props = defineProps<Props>()

const widgetLeft = ref(0);
const widgetTop = ref(0)

const widgetStyles = computed(() => {
    return {
        left: `${widgetLeft.value}px`,
        top: `${widgetTop.value}px`
    }
})

const isDragging = ref(false)
const startX = ref(0)
const startY = ref(0)

const dragStarted = (e: DragEvent) => {
    isDragging.value = true;
    startX.value = e.x;
    startY.value = e.y
    // console.log('starting y: ', startY.value);
}

onMounted(() => {
    window.addEventListener('dragover', (e: DragEvent) => {
        if(isDragging.value) {

            const xIncrement = e.x - startX.value;
            const yIncrement = e.y - startY.value;
            
            startY.value = startY.value + yIncrement;

            if(props.parentBoundaries.right > imageRef.value.getBoundingClientRect().right) {
                startX.value = startX.value + xIncrement;
                widgetLeft.value = widgetLeft.value + xIncrement;
            }
            widgetTop.value = widgetTop.value + yIncrement;
            // console.log('incremented left value: ', widgetLeft.value);

        }
    })
})
</script>

<style lang='postcss' scoped></style>