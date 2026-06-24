<script setup lang="ts">
import { computed } from 'vue';

interface ThermoProps {
  title: string;
  currentValue: number;
  targetValue: number;
  suffix?: string;
}

const props = withDefaults(defineProps<ThermoProps>(), {
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
</script>

<template>
  <div class="thermo-widget">
    <h3 class="thermo-title">{{ props.title }}</h3>

    <div class="thermo-layout">
      <div class="thermo-glass">
        <div class="thermo-liquid" :style="{ height: percentage + '%', backgroundColor: statusColor }"></div>
      </div>

      <div class="thermo-data">
        <div class="data-block target">
          <span class="label">Meta</span>
          <strong>{{ props.suffix }}{{ props.targetValue }}</strong>
        </div>

        <div class="data-block current" :style="{ color: statusColor }">
          <span class="percentage">{{ percentage }}%</span>
          <strong>{{ props.suffix }}{{ props.currentValue }}</strong>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.thermo-widget {
  background-color: #1e293b;
  border-radius: 16px;
  padding: 24px;
  width: 240px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  font-family: sans-serif;
  color: #f8fafc;
}

.thermo-title {
  margin: 0 0 20px 0;
  text-align: center;
  font-size: 1.1rem;
  color: #cbd5e1;
}

.thermo-layout {
  display: flex;
  align-items: stretch;
  gap: 20px;
  height: 160px;
}

.thermo-glass {
  width: 24px;
  background-color: #334155;
  border-radius: 20px;
  position: relative;
  display: flex;
  align-items: flex-end;
  padding: 4px;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.5);
}

.thermo-liquid {
  width: 100%;
  border-radius: 16px;
  transition: height 0.6s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  position: relative;
}

.thermo-liquid::after {
  content: '';
  position: absolute;
  bottom: -10px;
  left: 50%;
  transform: translateX(-50%);
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: inherit;
  box-shadow: inherit;
}

.thermo-data {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  flex: 1;
  padding-bottom: 10px;
}

.data-block {
  display: flex;
  flex-direction: column;
}

.data-block .label {
  font-size: 0.8rem;
  color: #94a3b8;
}

.data-block.target {
  align-items: flex-start;
  border-bottom: 1px dashed #475569;
  padding-bottom: 8px;
}

.data-block.current {
  align-items: flex-start;
}

.data-block .percentage {
  font-size: 1.8rem;
  font-weight: bold;
  line-height: 1.1;
}
</style>