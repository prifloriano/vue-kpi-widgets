<script setup lang="ts">
import { ref, computed } from 'vue';

interface SeriesData {
  name: string;
  data: number[];
  color: string;
}

interface SplineProps {
  title: string;
  categories: string[];
  series: SeriesData[];
}

const props = defineProps<SplineProps>();

const hiddenSeries = ref<string[]>([]);

const toggleLegend = (name: string): void => {
  if (hiddenSeries.value.includes(name)) {
    hiddenSeries.value = hiddenSeries.value.filter(n => n !== name);
  } else {
    hiddenSeries.value.push(name);
  }
};

const visibleSeries = computed(() => {
  return props.series.filter(s => !hiddenSeries.value.includes(s.name));
});

const maxValue = computed(() => {
  if (visibleSeries.value.length === 0) return 100;
  let max = 0;
  visibleSeries.value.forEach(s => {
    const sMax = Math.max(...s.data);
    if (sMax > max) max = sMax;
  });
  return max * 1.1;
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

const generatePaths = (data: number[]) => {
  if (data.length === 0) return { line: '', area: '' };

  const xStep = 100 / (data.length - 1);
  const getX = (index: number) => index * xStep;
  const getY = (val: number) => 100 - ((val / maxValue.value) * 100);

  let path = `M 0,${getY(data[0])}`;

  for (let i = 1; i < data.length; i++) {
    const xPrev = getX(i - 1);
    const yPrev = getY(data[i - 1]);
    const xCurr = getX(i);
    const yCurr = getY(data[i]);
    const cpX = (xPrev + xCurr) / 2;
    path += ` C ${cpX},${yPrev} ${cpX},${yCurr} ${xCurr},${yCurr}`;
  }

  return {
    line: path,
    area: `${path} L 100,100 L 0,100 Z`
  };
};

const chartedSeries = computed(() => {
  return visibleSeries.value.map(s => ({
    ...s,
    paths: generatePaths(s.data)
  }));
});
</script>

<template>
  <div class="spline-widget">
    <div class="spline-header">
      <h3 class="spline-title">{{ props.title }}</h3>

      <div class="spline-legend">
        <div v-for="s in props.series" :key="s.name" class="legend-item"
          :class="{ 'is-hidden': hiddenSeries.includes(s.name) }" @click="toggleLegend(s.name)">
          <span class="legend-color" :style="{ backgroundColor: s.color }"></span>
          {{ s.name }}
        </div>
      </div>
    </div>

    <div class="chart-body">

      <div class="y-axis">
        <span v-for="(marker, index) in yAxisMarkers" :key="'y-' + index" class="y-label">
          {{ marker }}
        </span>
      </div>

      <div class="chart-area">
        <svg viewBox="0 0 100 100" class="spline-svg" preserveAspectRatio="none">
          <line x1="0" y1="25" x2="100" y2="25" stroke="#e2e8f0" stroke-width="0.3" />
          <line x1="0" y1="50" x2="100" y2="50" stroke="#e2e8f0" stroke-width="0.3" />
          <line x1="0" y1="75" x2="100" y2="75" stroke="#e2e8f0" stroke-width="0.3" />

          <g v-for="s in chartedSeries" :key="'series-' + s.name" class="animated-group">
            <path :d="s.paths.area" :fill="s.color" opacity="0.5" class="area-path" />
            <path :d="s.paths.line" fill="none" :stroke="s.color" stroke-width="1.5" class="line-path" />
          </g>
        </svg>
      </div>

    </div>

    <div class="x-axis">
      <span v-for="(cat, index) in props.categories" :key="index">{{ cat }}</span>
    </div>
  </div>
</template>

<style scoped>
.spline-widget {
  background-color: #ffffff;
  border-radius: 8px;
  padding: 24px;
  width: 100%;
  max-width: 800px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
  font-family: sans-serif;
  color: #334155;
  border: 1px solid #e2e8f0;
}

.spline-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.spline-title {
  margin: 0;
  font-size: 1.2rem;
  font-weight: 600;
  color: #475569;
}

.spline-legend {
  display: flex;
  gap: 15px;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.8rem;
  color: #64748b;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
  padding: 4px 8px;
  border-radius: 4px;
}

.legend-item:hover {
  background-color: #f1f5f9;
  color: #334155;
}

.legend-item.is-hidden {
  opacity: 0.4;
  text-decoration: line-through;
}

.legend-color {
  width: 16px;
  height: 4px;
  border-radius: 2px;
}

.chart-body {
  display: flex;
  height: 250px;
  gap: 12px;
}

.y-axis {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  color: #94a3b8;
  font-size: 0.75rem;
  text-align: right;
  width: 35px;
  padding-bottom: 2px;
  font-weight: 500;
}

.y-label {
  line-height: 0;
}

.chart-area {
  flex: 1;
  position: relative;
  border-bottom: 1px solid #e2e8f0;
  border-left: 1px solid #e2e8f0;
}

.spline-svg {
  width: 100%;
  height: 100%;
  overflow: visible;
}

.area-path,
.line-path {
  transition: d 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}

.animated-group {
  transform-origin: bottom;
  animation: waveRise 1.2s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

.animated-group:nth-child(2) {
  animation-delay: 0.2s;
}

@keyframes waveRise {
  0% {
    transform: scaleY(0);
    opacity: 0;
  }

  100% {
    transform: scaleY(1);
    opacity: 1;
  }
}

.x-axis {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
  margin-left: 47px;
  font-size: 0.75rem;
  color: #94a3b8;
  font-weight: 500;
}
</style>