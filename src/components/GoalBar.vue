<script setup lang="ts">
import { computed } from 'vue';

interface BarProps {
  title: string;
  currentValue: number;
  targetValue: number;
  suffix?: string;
}

const props = withDefaults(defineProps<BarProps>(), {
  suffix: ''
});

const percentage = computed(() => {
  const calc = (props.currentValue / props.targetValue) * 100;
  return calc > 100 ? 100 : Math.round(calc);
});

const statusColor = computed(() => {
  if (percentage.value < 50) return '#ef4444';
  if (percentage.value < 80) return '#f59e0b';
  return '#10b981'; // Verde
});
</script>

<template>
  <div class="bar-widget">
    <div class="bar-header">
      <span class="bar-title">{{ props.title }}</span>
      <span class="bar-percentage" :style="{ color: statusColor }">{{ percentage }}%</span>
    </div>

    <div class="bar-track">
      <div class="bar-fill"
        :style="{ width: percentage + '%', backgroundColor: statusColor, boxShadow: `0 0 10px ${statusColor}` }"></div>
    </div>

    <div class="bar-footer">
      <span>{{ props.suffix }} {{ props.currentValue }}</span>
      <span>Meta: {{ props.suffix }} {{ props.targetValue }}</span>
    </div>
  </div>
</template>

<style scoped>
.bar-widget {
  background-color: #1e293b;
  border-radius: 12px;
  padding: 20px;
  width: 320px;
  font-family: sans-serif;
  color: #f8fafc;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.bar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.bar-title {
  font-weight: 600;
  color: #cbd5e1;
}

.bar-percentage {
  font-weight: bold;
  font-size: 1.1rem;
}

.bar-track {
  width: 100%;
  height: 12px;
  background-color: #334155;
  border-radius: 6px;
  overflow: hidden;
  margin-bottom: 10px;
}

.bar-fill {
  height: 100%;
  border-radius: 6px;
  transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.bar-footer {
  display: flex;
  justify-content: space-between;
  font-size: 0.85rem;
  color: #94a3b8;
}
</style>