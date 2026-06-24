<script setup lang="ts">
import { computed } from 'vue';

interface RingData {
  label: string;
  value: number;
  targetValue: number;
  color: string;
}

interface RingsProps {
  title: string;
  data: RingData[];
}

const props = defineProps<RingsProps>();

const size = 200;
const center = size / 2;
const strokeWidth = 12;
const gap = 4;
const maxRadius = 85;

const rings = computed(() => {
  return props.data.map((item, index) => {
    const percentage = Math.min((item.value / item.targetValue) * 100, 100);

    const radius = maxRadius - (index * (strokeWidth + gap));

    const circumference = 2 * Math.PI * radius;

    const dashoffset = circumference - (percentage / 100) * circumference;

    return {
      ...item,
      percentage: Math.round(percentage),
      radius,
      circumference,
      dashoffset
    };
  });
});
</script>

<template>
  <div class="rings-widget">
    <h3 class="rings-title">{{ props.title }}</h3>

    <div class="rings-container">
      <svg :width="size" :height="size" :viewBox="`0 0 ${size} ${size}`" class="rings-svg">

        <circle v-for="(ring, index) in rings" :key="'bg-' + index" :cx="center" :cy="center" :r="ring.radius"
          fill="none" stroke="#334155" :stroke-width="strokeWidth" />

        <circle v-for="(ring, index) in rings" :key="'progress-' + index" :cx="center" :cy="center" :r="ring.radius"
          fill="none" :stroke="ring.color" :stroke-width="strokeWidth" stroke-linecap="round"
          :stroke-dasharray="ring.circumference" :stroke-dashoffset="ring.dashoffset" class="ring-progress" />
      </svg>

      <div class="rings-legend">
        <div v-for="(ring, index) in rings" :key="'legend-' + index" class="legend-item">
          <div class="legend-header">
            <span class="legend-dot" :style="{ backgroundColor: ring.color }"></span>
            <span class="legend-label">{{ ring.label }}</span>
          </div>
          <span class="legend-value">{{ ring.percentage }}% ({{ ring.value }})</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.rings-widget {
  background-color: #1e293b;
  border-radius: 16px;
  padding: 24px;
  width: 100%;
  max-width: 380px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  font-family: sans-serif;
  color: #f8fafc;
}

.rings-title {
  margin: 0 0 20px 0;
  text-align: center;
  font-size: 1.1rem;
  color: #cbd5e1;
}

.rings-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
}

.rings-svg {
  transform: rotate(-90deg);
  overflow: visible;
  filter: drop-shadow(0 4px 6px rgba(0, 0, 0, 0.3));
}

.ring-progress {
  transition: stroke-dashoffset 1s cubic-bezier(0.4, 0, 0.2, 1);
}

.rings-legend {
  display: flex;
  flex-direction: column;
  gap: 12px;
  flex: 1;
}

.legend-item {
  display: flex;
  flex-direction: column;
  background-color: #0f172a;
  padding: 8px 12px;
  border-radius: 8px;
  border-left: 3px solid transparent;
}

.legend-header {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 4px;
}

.legend-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
}

.legend-label {
  font-size: 0.8rem;
  color: #94a3b8;
  font-weight: 600;
}

.legend-value {
  font-size: 1.1rem;
  font-weight: bold;
  color: #f8fafc;
  padding-left: 18px;
}
</style>