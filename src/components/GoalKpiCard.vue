<script setup lang="ts">
import { computed } from 'vue';

interface KpiCardProps {
  title: string;
  value: string | number;
  percentageDiff?: number;
  diffPeriod?: string;
}

const props = defineProps<KpiCardProps>();

const isPositive = computed(() => (props.percentageDiff ?? 0) > 0);
const isNegative = computed(() => (props.percentageDiff ?? 0) < 0);
const isNeutral = computed(() => (props.percentageDiff ?? 0) === 0);

const absDiff = computed(() => {
  if (props.percentageDiff === undefined) return 0;
  return Math.abs(props.percentageDiff);
});
</script>

<template>
  <div class="kpi-card-widget">
    <span class="kpi-title">{{ props.title }}</span>

    <div class="kpi-value">{{ props.value }}</div>

    <div v-if="props.percentageDiff !== undefined" class="kpi-trend">
      <span class="trend-badge" :class="{ 'positive': isPositive, 'negative': isNegative, 'neutral': isNeutral }">
        <span v-if="isPositive">▲</span>
        <span v-if="isNegative">▼</span>
        {{ absDiff }}%
      </span>
      <span class="trend-period">{{ props.diffPeriod }}</span>
    </div>
  </div>
</template>

<style scoped>
.kpi-card-widget {
  background-color: #1e293b;
  border-radius: 16px;
  padding: 20px 24px;
  width: 100%;
  min-width: 220px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  font-family: sans-serif;
  display: flex;
  flex-direction: column;
  gap: 6px;
  border: 1px solid #334155;
  transition: transform 0.2s ease, border-color 0.2s ease;
}

.kpi-card-widget:hover {
  transform: translateY(-2px);
  border-color: #475569;
}

.kpi-title {
  font-size: 0.85rem;
  color: #94a3b8;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.kpi-value {
  font-size: 2.2rem;
  font-weight: 800;
  color: #f8fafc;
  line-height: 1.1;
}

.kpi-trend {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 4px;
  font-size: 0.8rem;
}

.trend-badge {
  font-weight: bold;
  display: flex;
  align-items: center;
  gap: 3px;
}

.trend-badge.positive {
  color: #10b981;
}

.trend-badge.negative {
  color: #f43f5e;
}

.trend-badge.neutral {
  color: #94a3b8;
}

.trend-period {
  color: #64748b;
}
</style>