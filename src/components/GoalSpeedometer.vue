<script setup lang="ts">
import { computed } from 'vue';

interface SpeedometerProps {
  title: string;
  currentValue: number;
  targetValue: number;
  suffix?: string;
}

const props = withDefaults(defineProps<SpeedometerProps>(), {
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

const visualPercentage = computed(() => percentage.value / 2);

const conicStyle = computed(() => {
  return {
    background: `conic-gradient(from 270deg, ${statusColor.value} 0%, ${statusColor.value} ${visualPercentage.value}%, #334155 ${visualPercentage.value}%, #334155 50%, transparent 50%)`
  };
});
</script>

<template>
  <div class="speed-widget">
    <h3 class="speed-title">{{ props.title }}</h3>

    <div class="speed-container">
      <div class="speed-box">
        <div class="speed-ring" :style="conicStyle">
          <div class="speed-inner">
            <span class="percentage-text" :style="{ color: statusColor }">
              {{ percentage }}%
            </span>
          </div>
        </div>
      </div>
    </div>

    <div class="speed-footer">
      <span>Atual: <strong>{{ props.suffix }}{{ props.currentValue }}</strong></span>
      <span>Meta: <strong>{{ props.suffix }}{{ props.targetValue }}</strong></span>
    </div>
  </div>
</template>

<style scoped>
.speed-widget {
  background-color: #1e293b;
  border-radius: 16px;
  padding: 24px;
  width: 280px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  font-family: sans-serif;
  color: #f8fafc;
}

.speed-title {
  margin: 0 0 20px 0;
  text-align: center;
  font-size: 1.1rem;
  color: #cbd5e1;
}

.speed-container {
  display: flex;
  justify-content: center;
  margin-bottom: 10px;
}

.speed-box {
  width: 160px;
  height: 80px;
  position: relative;
  overflow: hidden;
}

.speed-ring {
  width: 160px;
  height: 160px;
  border-radius: 50%;
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.speed-inner {
  width: 120px;
  height: 120px;
  background-color: #1e293b;
  border-radius: 50%;
  margin-top: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: inset 0 2px 10px rgba(0, 0, 0, 0.5);
}

.percentage-text {
  font-size: 2rem;
  font-weight: bold;
  transform: translateY(-20px);
}

.speed-footer {
  display: flex;
  justify-content: space-between;
  font-size: 0.85rem;
  color: #94a3b8;
  border-top: 1px solid #334155;
  padding-top: 12px;
}
</style>