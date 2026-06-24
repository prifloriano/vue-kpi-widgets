<script setup lang="ts">
import { computed } from 'vue';

interface CircleProps {
  title: string;
  currentValue: number;
  targetValue: number;
  suffix?: string;
}

const props = withDefaults(defineProps<CircleProps>(), {
  suffix: ''
});

const percentage = computed(() => {
  const calc = (props.currentValue / props.targetValue) * 100;
  return calc > 100 ? 100 : Math.round(calc);
});

const statusColor = computed(() => {
  if (percentage.value < 50) return '#ef4444';
  if (percentage.value < 80) return '#f59e0b';
  return '#10b981';
});

const conicStyle = computed(() => {
  return {
    background: `conic-gradient(${statusColor.value} ${percentage.value}%, #334155 ${percentage.value}%)`,
    boxShadow: `0 0 20px ${statusColor.value}30`
  };
});
</script>

<template>
  <div class="circle-widget">
    <h3 class="circle-title">{{ props.title }}</h3>

    <div class="gauge-container">
      <div class="gauge-ring" :style="conicStyle">
        <div class="gauge-inner">
          <span class="percentage-text" :style="{ color: statusColor }">
            {{ percentage }}%
          </span>
        </div>
      </div>
    </div>

    <div class="circle-footer">
      <span>Atual: <strong>{{ props.suffix }}{{ props.currentValue }}</strong></span>
      <span>Meta: <strong>{{ props.suffix }}{{ props.targetValue }}</strong></span>
    </div>
  </div>
</template>

<style scoped>
.circle-widget {
  background-color: #1e293b;
  border-radius: 16px;
  padding: 24px;
  width: 280px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  font-family: sans-serif;
  color: #f8fafc;
}

.circle-title {
  margin: 0 0 20px 0;
  text-align: center;
  font-size: 1.1rem;
  color: #cbd5e1;
  font-weight: 600;
}

.gauge-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.gauge-ring {
  width: 140px;
  height: 140px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.5s ease-in-out;
}

.gauge-inner {
  width: 110px;
  height: 110px;
  background-color: #1e293b;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  /* Um sombreado interno pra dar profundidade no miolo */
  box-shadow: inset 0 2px 10px rgba(0, 0, 0, 0.5);
}

.percentage-text {
  font-size: 2rem;
  font-weight: bold;
}

.circle-footer {
  display: flex;
  justify-content: space-between;
  font-size: 0.85rem;
  color: #94a3b8;
  border-top: 1px solid #334155;
  padding-top: 12px;
}
</style>