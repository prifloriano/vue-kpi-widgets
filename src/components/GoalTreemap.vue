<script setup lang="ts">
import { computed } from 'vue';

interface TreemapData {
  label: string;
  value: number;
  color?: string;
}

interface TreemapProps {
  title: string;
  data: TreemapData[];
}

const props = defineProps<TreemapProps>();

const sortedData = computed(() => {
  return [...props.data].sort((a, b) => b.value - a.value);
});

const totalValue = computed(() => {
  return sortedData.value.reduce((sum, item) => sum + item.value, 0);
});

const treemapCells = computed(() => {
  let currentX = 0;
  let currentY = 0;
  let currentW = 100;
  let currentH = 100;
  let remainingTotal = totalValue.value;
  let isVerticalSplit = true;

  return sortedData.value.map((item, index) => {
    const fraction = remainingTotal === 0 ? 0 : item.value / remainingTotal;

    let rectW = 0, rectH = 0, nextX = 0, nextY = 0;

    if (index === sortedData.value.length - 1) {
      rectW = currentW;
      rectH = currentH;
    } else {
      if (isVerticalSplit) {
        rectW = currentW * fraction;
        rectH = currentH;
        nextX = currentX + rectW;
        nextY = currentY;
        currentW -= rectW;
      } else {
        rectW = currentW;
        rectH = currentH * fraction;
        nextX = currentX;
        nextY = currentY + rectH;
        currentH -= rectH;
      }
    }

    const cell = {
      ...item,
      x: currentX,
      y: currentY,
      w: rectW,
      h: rectH
    };

    if (index !== sortedData.value.length - 1) {
      currentX = nextX;
      currentY = nextY;
      remainingTotal -= item.value;
      isVerticalSplit = !isVerticalSplit;
    }

    return cell;
  });
});
</script>

<template>
  <div class="treemap-widget">
    <h3 class="treemap-title">{{ props.title }}</h3>

    <div class="treemap-container">

      <div v-for="(cell, index) in treemapCells" :key="index" class="treemap-cell" :style="{
        left: cell.x + '%',
        top: cell.y + '%',
        width: cell.w + '%',
        height: cell.h + '%',
        backgroundColor: cell.color || '#3b82f6'
      }">
        <div class="cell-content">
          <span class="cell-label">{{ cell.label }}</span>
          <span class="cell-value">{{ cell.value }}</span>
        </div>
      </div>

    </div>
  </div>
</template>

<style scoped>
.treemap-widget {
  background-color: #1e293b;
  border-radius: 16px;
  padding: 24px;
  width: 100%;
  max-width: 450px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  font-family: sans-serif;
  color: #f8fafc;
}

.treemap-title {
  margin: 0 0 16px 0;
  text-align: center;
  font-size: 1.1rem;
  color: #cbd5e1;
}

.treemap-container {
  position: relative;
  width: 100%;
  height: 250px;
  background-color: #0f172a;
  border-radius: 8px;
  overflow: hidden;
  border: 2px solid #0f172a;
}

.treemap-cell {
  position: absolute;
  border: 1px solid #1e293b;
  box-sizing: border-box;
  transition: filter 0.3s ease;
  cursor: pointer;
  display: flex;
  align-items: flex-end;
}

.treemap-cell:hover {
  filter: brightness(1.1);
}

.cell-content {
  padding: 8px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.cell-label {
  font-size: 0.75rem;
  font-weight: 600;
  opacity: 0.9;
}

.cell-value {
  font-size: 1rem;
  font-weight: bold;
}
</style>