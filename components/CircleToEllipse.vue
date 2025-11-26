<template>
  <div class="circle-to-ellipse">
    <div class="controls-top">
      <button @click="toggleAnimation" class="btn">
        {{ isAnimating ? 'Pause' : (progress >= 1 ? 'Restart' : 'Start') }}
      </button>
      <div class="progress-label">
        {{ progress < 0.01 ? 'Circle' : progress >= 0.99 ? 'Ellipse (4 extrema marked)' : 'Transforming...' }}
      </div>
    </div>

    <svg :width="width" :height="height" :viewBox="`0 0 ${width} ${height}`">
      <!-- Ellipse -->
      <ellipse
        :cx="centerX"
        :cy="centerY"
        :rx="rx"
        :ry="ry"
        fill="none"
        stroke="#4a90e2"
        stroke-width="3"
      />

      <!-- Extrema points (show as transformation progresses) -->
      <g v-if="progress > 0.1">
        <!-- Top extremum (maximum curvature) -->
        <circle
          :cx="centerX"
          :cy="centerY - ry"
          r="6"
          fill="#ff6b6b"
          :opacity="Math.min(1, (progress - 0.1) * 2)"
        />

        <!-- Bottom extremum (maximum curvature) -->
        <circle
          :cx="centerX"
          :cy="centerY + ry"
          r="6"
          fill="#ff6b6b"
          :opacity="Math.min(1, (progress - 0.1) * 2)"
        />

        <!-- Left extremum (minimum curvature) -->
        <circle
          :cx="centerX - rx"
          :cy="centerY"
          r="6"
          fill="#51cf66"
          :opacity="Math.min(1, (progress - 0.1) * 2)"
        />

        <!-- Right extremum (minimum curvature) -->
        <circle
          :cx="centerX + rx"
          :cy="centerY"
          r="6"
          fill="#51cf66"
          :opacity="Math.min(1, (progress - 0.1) * 2)"
        />
      </g>
    </svg>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  width: {
    type: Number,
    default: 600
  },
  height: {
    type: Number,
    default: 400
  },
  duration: {
    type: Number,
    default: 3000 // milliseconds
  },
  autoStart: {
    type: Boolean,
    default: false
  }
})

const progress = ref(0)
const isAnimating = ref(false)
let animationFrame = null
let startTime = null

const centerX = computed(() => props.width / 2)
const centerY = computed(() => props.height / 2)

// Start with a circle (equal radii)
const baseRadius = computed(() => Math.min(props.width, props.height) * 0.3)

// Interpolate from circle to ellipse
const rx = computed(() => {
  const circleR = baseRadius.value
  const ellipseRx = baseRadius.value * 1.5
  return circleR + (ellipseRx - circleR) * progress.value
})

const ry = computed(() => {
  const circleR = baseRadius.value
  const ellipseRy = baseRadius.value * 0.7
  return circleR + (ellipseRy - circleR) * progress.value
})

const animate = (timestamp) => {
  if (!startTime) startTime = timestamp
  const elapsed = timestamp - startTime

  progress.value = Math.min(elapsed / props.duration, 1)

  if (progress.value < 1) {
    animationFrame = requestAnimationFrame(animate)
  } else {
    isAnimating.value = false
    startTime = null
  }
}

const toggleAnimation = () => {
  if (progress.value >= 1) {
    // Restart
    progress.value = 0
    startTime = null
  }

  if (isAnimating.value) {
    // Pause
    isAnimating.value = false
    if (animationFrame) {
      cancelAnimationFrame(animationFrame)
      animationFrame = null
    }
    startTime = null
  } else {
    // Start/Resume
    isAnimating.value = true
    animationFrame = requestAnimationFrame(animate)
  }
}

onMounted(() => {
  if (props.autoStart) {
    toggleAnimation()
  }
})

onUnmounted(() => {
  if (animationFrame) {
    cancelAnimationFrame(animationFrame)
  }
})
</script>

<style scoped>
.circle-to-ellipse {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
}

.controls-top {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 0;
}

.btn {
  padding: 0.4rem 1rem;
  font-size: 0.9rem;
  background: #4a90e2;
  color: white;
  border: none;
  border-radius: 0.4rem;
  cursor: pointer;
  transition: background 0.2s;
  white-space: nowrap;
}

.btn:hover {
  background: #357abd;
}

.progress-label {
  font-size: 0.9rem;
  color: #666;
  font-weight: 500;
}
</style>
