<script setup lang="ts">
import { computed } from 'vue';

interface FunnelData {
  label: string;
  value: number;
  color?: string;
}

interface FunnelProps {
  title: string;
  data: FunnelData[];
}

const props = defineProps<FunnelProps>();

const maxValue = computed(() => {
  if (props.data.length === 0) return 100;
  return Math.max(...props.data.map(item => item.value));
});

const sortedData = computed(() => {
  return [...props.data].sort((a, b) => b.value - a.value);
});

const formatNumber = (num: number) => {
  if (num >= 1000) return (num / 1000).toFixed(1).replace('.0', '') + 'k';
  return num.toString();
};

const getLayerWidth = (value: number) => {
  if (maxValue.value === 0) return 0;
  return (value / maxValue.value) * 100;
};
</script>

<template>
  <div class="funnel-widget">
    <h3 class="funnel-title">{{ props.title }}</h3>

    <div class="funnel-container">
      <div v-for="(item, index) in sortedData" :key="index" class="funnel-layer-wrapper">
        <div class="layer-label">{{ item.label }}</div>

        <div class="layer-block-container">
          <div class="layer-block" :style="{
            width: getLayerWidth(item.value) + '%',
            backgroundColor: item.color || '#9f1239'
          }">
            <span class="layer-value">{{ formatNumber(item.value) }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.funnel-widget {
  background-color: #1e293b;
  border-radius: 16px;
  padding: 24px;
  width: 100%;
  max-width: 400px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  font-family: sans-serif;
  color: #f8fafc;
}

.funnel-title {
  margin: 0 0 20px 0;
  text-align: center;
  font-size: 1.1rem;
  color: #cbd5e1;
}

.funnel-container {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.funnel-layer-wrapper {
  display: flex;
  align-items: center;
  width: 100%;
}

.layer-label {
  width: 30%;
  font-size: 0.8rem;
  color: #94a3b8;
  text-align: right;
  padding-right: 15px;
}

.layer-block-container {
  width: 70%;
  display: flex;
  justify-content: center;
}

.layer-block {
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  transition: width 0.6s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: pointer;
  position: relative;
}

.layer-block:hover {
  filter: brightness(1.2);
}

.layer-block::after {
  content: '';
  position: absolute;
  bottom: -6px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 6px solid transparent;
  border-right: 6px solid transparent;
  border-top: 6px solid inherit;
  opacity: 0.8;
}

.funnel-layer-wrapper:last-child .layer-block::after {
  display: none;
}

.layer-value {
  font-weight: bold;
  font-size: 0.9rem;
  color: #ffffff;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
}
</style>