<template>
    <div ref="blockRef" class="w-full h-96 border-2 relative">
        <Image :parent-boundaries="boundaries" />
    </div>
</template>

<script lang='ts' setup>
import { onMounted, ref } from 'vue';
import Image from './image.vue';
const boundaries = ref()
const blockRef = ref()
onMounted(() => {
    boundaries.value = blockRef.value.getBoundingClientRect()
})
</script>

<style lang='postcss' scoped></style>