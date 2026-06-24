<script setup lang="ts">
import { ref, computed } from 'vue';

interface ColumnData {
  label: string;
  value: number;
  color?: string;
}

interface ColumnProps {
  title: string;
  data: ColumnData[];
}

const props = defineProps<ColumnProps>();
const hiddenLabels = ref<string[]>([]);

const toggleLegend = (label: string): void => {
  if (hiddenLabels.value.includes(label)) {
    hiddenLabels.value = hiddenLabels.value.filter(l => l !== label);
  } else {
    hiddenLabels.value.push(label);
  }
};

const visibleData = computed(() => {
  return props.data.filter(item => !hiddenLabels.value.includes(item.label));
});

const maxValue = computed(() => {
  if (visibleData.value.length === 0) return 100;
  return Math.max(...visibleData.value.map(item => item.value));
});

const formatNumber = (num: number) => {
  if (num >= 1000) return (num / 1000).toFixed(1).replace('.0', '') + 'k';
  return Math.round(num).toString();
};

const yAxisMarkers = computed(() => {
  const max = maxValue.value;
  return [
    formatNumber(max),
    formatNumber(max * 0.75),
    formatNumber(max * 0.5),
    formatNumber(max * 0.25),
    '0'
  ];
});

const barWidth = computed(() => {
  if (visibleData.value.length === 0) return 0;
  return 100 / visibleData.value.length;
});

const getBarHeight = (value: number) => {
  if (maxValue.value === 0) return 0;
  return (value / maxValue.value) * 100;
};
</script>

<template>
  <div class="custom-chart-widget">
    <h3 class="chart-title">{{ props.title }}</h3>

    <div class="chart-body">

      <div class="y-axis">
        <span v-for="(marker, index) in yAxisMarkers" :key="'y-' + index" class="y-label">
          {{ marker }}
        </span>
      </div>

      <div class="svg-container">
        <svg viewBox="0 0 100 100" class="chart-svg" preserveAspectRatio="none">
          <line x1="0" y1="0" x2="100" y2="0" stroke="#334155" stroke-width="0.5" stroke-dasharray="2" />
          <line x1="0" y1="25" x2="100" y2="25" stroke="#334155" stroke-width="0.5" stroke-dasharray="2" />
          <line x1="0" y1="50" x2="100" y2="50" stroke="#334155" stroke-width="0.5" stroke-dasharray="2" />
          <line x1="0" y1="75" x2="100" y2="75" stroke="#334155" stroke-width="0.5" stroke-dasharray="2" />

          <g v-for="(item, index) in visibleData" :key="'rect-' + item.label">
            <rect :x="(index * barWidth) + (barWidth * 0.2)" :y="100 - getBarHeight(item.value)" :width="barWidth * 0.6"
              :height="getBarHeight(item.value)" :fill="item.color || '#3b82f6'" rx="2" class="bar-rect">
              <title>{{ item.label }}: {{ item.value }}</title>
            </rect>
          </g>
        </svg>
      </div>

    </div>

    <div class="x-axis">
      <span v-for="item in visibleData" :key="'axis-' + item.label">
        {{ item.label }}
      </span>
    </div>

    <div class="legend-container">
      <div v-for="item in props.data" :key="'legenda-' + item.label" class="legend-item"
        :class="{ 'is-hidden': hiddenLabels.includes(item.label) }" @click="toggleLegend(item.label)">
        <span class="legend-color" :style="{ backgroundColor: item.color || '#3b82f6' }"></span>
        {{ item.label }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.custom-chart-widget {
  background-color: #1e293b;
  border-radius: 16px;
  padding: 24px;
  width: 100%;
  max-width: 500px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  font-family: sans-serif;
  color: #f8fafc;
}

.chart-title {
  margin: 0 0 20px 0;
  font-size: 1.1rem;
  color: #cbd5e1;
}

.chart-body {
  display: flex;
  height: 200px;
  gap: 15px;
}

.y-axis {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  color: #64748b;
  font-size: 0.75rem;
  text-align: right;
  width: 35px;
  padding-bottom: 2px;
  padding-top: -5px;
}

.y-label {
  line-height: 0;
}

.svg-container {
  flex: 1;
  height: 100%;
  border-bottom: 1px solid #475569;
}

.chart-svg {
  width: 100%;
  height: 100%;
  overflow: visible;
}

.bar-rect {
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: pointer;
}

.bar-rect:hover {
  opacity: 0.8;
}

.x-axis {
  display: flex;
  justify-content: space-around;
  margin-top: 10px;
  margin-left: 50px;
  font-size: 0.8rem;
  color: #94a3b8;
  height: 20px;
}

.legend-container {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  justify-content: center;
  margin-top: 20px;
  padding-top: 15px;
  border-top: 1px dashed #334155;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.85rem;
  color: #e2e8f0;
  cursor: pointer;
  transition: all 0.3s ease;
  padding: 4px 8px;
  border-radius: 6px;
}

.legend-item:hover {
  background-color: #334155;
}

.legend-item.is-hidden {
  opacity: 0.4;
  text-decoration: line-through;
}

.legend-color {
  width: 12px;
  height: 12px;
  border-radius: 4px;
}
</style>