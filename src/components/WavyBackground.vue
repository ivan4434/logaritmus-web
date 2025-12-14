<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue'

const props = defineProps({
    colors: {
        type: Array,
        required: true,
    },
    reverse: {
        type: Boolean,
        default: false
    },
    speed: {
        type: Number,
        default: 4000
    },
    phaseOffset: {
        type: Number,
        default: 0
    }
})

const canvasRef = ref(null)
let animationId = null
let startTime = null

const draw = (timestamp) => {
    const canvas = canvasRef.value
    if (!canvas) return

    const ctx = canvas.getContext('2d')
    const width = canvas.width
    const height = canvas.height

    if (!startTime) startTime = timestamp
    const elapsed = timestamp - startTime
    
    const phase = (Math.PI * 2 * ((elapsed % props.speed) / props.speed))

    ctx.clearRect(0, 0, width, height)

    const amplitude1 = height / 6
    const amplitude2 = height / 8

    ctx.beginPath()

    const startY = props.reverse ? 0 : height
    ctx.moveTo(0, startY)

    const maxAmplitude = amplitude1 + amplitude2

    for (let x = 0; x <= width; x += 10) {
        const y1 = amplitude1 * Math.sin((x / 150) + phase + props.phaseOffset)
        const y2 = amplitude2 * Math.sin((x / 80) - phase + props.phaseOffset)
        const y = y1 + y2

        const baseHeight = height - maxAmplitude

        const finalY = props.reverse ? y : baseHeight - y
        ctx.lineTo(x, finalY)
    }

    if (props.reverse) {
        ctx.lineTo(width, height)
        ctx.lineTo(0, height)
    } else {
        ctx.lineTo(width, 0)
        ctx.lineTo(0, 0)
    }
    
    ctx.closePath()

    const gradient = ctx.createLinearGradient(0, 0, 0, height)
    
    props.colors.forEach((color, index) => {
        const stop = index / (props.colors.length - 1)
        gradient.addColorStop(stop, color)
    })

    ctx.fillStyle = gradient
    ctx.fill()

    animationId = requestAnimationFrame(draw)
}

const handleResize = () => {
    if (canvasRef.value) {
        canvasRef.value.width = canvasRef.value.offsetWidth
        canvasRef.value.height = canvasRef.value.offsetHeight
    }
}

onMounted(() => {
    handleResize()
    window.addEventListener('resize', handleResize)
    animationId = requestAnimationFrame(draw)
})

onUnmounted(() => {
    window.removeEventListener('resize', handleResize)
    cancelAnimationFrame(animationId)
})
</script>

<template>
    <canvas 
        ref="canvasRef" 
        class="w-full h-full block"
    ></canvas>
</template>