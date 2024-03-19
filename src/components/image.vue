<template>
    <div ref="imageRef" class="absolute bg-yellow-300" draggable="true" @dragstart.stop="dragStarted" :style="widgetStyles">
        <img  class="w-full h-full object-cover"
            src="https://media.smartbloks.ai/media/__dld?__ct=image/png&__mxd=f75319bd-5f43-47ab-aa30-5600bbdcd87a/media/7a8d23ee-2d6a-4b79-bc28-0d24de3aba8b" />
        
        <!-- right -->
            <!-- <div draggable="true" @dragstart.stop="onRightResizeStart" class="absolute right-0 w-[0.12rem] cursor-ew-resize h-full flex top-0 bg-blue-600">
            <div class="h-6 w-6 rounded-full absolute top-[40%] -left-[10px] bg-blue-600"></div>
        </div> -->


        <!-- right bottom -->
        <div draggable="true" @dragstart.stop="onRightResizeStart" class="absolute right-0 w-[0.12rem] cursor-nwse-resize h-full flex top-0 bg-blue-600">
            <div class="h-6 w-6 rounded-full absolute bottom-[0%] -left-[10px] bg-blue-600"></div>
        </div>
    </div>
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
const widthValue = ref(100)
const heightValue = ref(200)

const widgetStyles = computed(() => {
    return {
        left: `${widgetLeft.value}px`,
        top: `${widgetTop.value}px`,
        width: `${widthValue.value}px`,
        height: `${heightValue.value}px`
    }
})

const isDragging = ref(false)
const isRightResizing = ref(false)

const rightBottomResizeStartX = ref(0)
const rightBottomResizeStartY = ref(0)

const startX = ref(0)
const startY = ref(0)

const onRightResizeStart = (e: DragEvent) => {
    isRightResizing.value = true;
    rightBottomResizeStartX.value = e.x
    rightBottomResizeStartY.value = e.y
}

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

        if(isRightResizing) {
            const xIncrement = e.x - rightBottomResizeStartX.value;
            const yIncrement = e.y - rightBottomResizeStartY.value

            rightBottomResizeStartX.value = rightBottomResizeStartX.value + xIncrement
            rightBottomResizeStartY.value = rightBottomResizeStartY.value + yIncrement
            widthValue.value = widthValue.value + xIncrement;
            heightValue.value = heightValue.value + yIncrement;
        }
    })
})
</script>

<style lang='postcss' scoped></style>