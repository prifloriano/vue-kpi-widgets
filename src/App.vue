<script setup lang="ts">
import GoalBar from './components/GoalBar.vue'
import GoalCircle from './components/GoalCircle.vue'
import GoalSpeedometer from './components/GoalSpeedometer.vue'
import GoalThermometer from './components/GoalThermometer.vue'
import GoalColumns from './components/GoalColumns.vue';
import GoalFunnel from './components/GoalFunnel.vue';
import GoalRings from './components/GoalRings.vue';
import GoalTreemap from './components/GoalTreemap.vue';
import GoalKpiCard from './components/GoalKpiCard.vue';
import GoalSplineArea from './components/GoalSplineArea.vue';
</script>

<template>
  <div class="dashboard">
    <header class="dash-header">
      <h1>Painel</h1>
    </header>

    <div class="widgets-grid">
      <GoalCircle title="Projetos de Inovação" :current-value="3" :target-value="5" />

      <GoalCircle title="Meta de Faturamento" :current-value="18500" :target-value="20000" suffix="R$ " />

      <GoalBar title="Aquisição de Clientes" :current-value="12" :target-value="30" />

      <GoalSpeedometer title="Saúde da Empresa" :current-value="8" :target-value="10" />

      <GoalThermometer title="Caixa Reserva" :current-value="25000" :target-value="50000" suffix="R$ " />

      <GoalColumns title="Vendas por Trimestre" :data="[
        { label: 'Q1', value: 4500, color: '#3b82f6' },
        { label: 'Q2', value: 8200, color: '#10b981' },
        { label: 'Q3', value: 6100, color: '#f59e0b' },
        { label: 'Q4', value: 11000, color: '#ef4444' }
      ]" />

      <GoalFunnel title="Funil de Contratações" :data="[
        { label: 'Temporário', value: 2000, color: '#be123c' },
        { label: 'Terceirizado', value: 800, color: '#9f1239' },
        { label: 'CLT', value: 5000, color: '#e11d48' },
        { label: 'Horista', value: 4000, color: '#f43f5e' }
      ]" />
      <GoalRings title="Metas de Campanha" :data="[
        { label: 'SEO e Orgânico', value: 85, targetValue: 100, color: '#10b981' },
        { label: 'Google Ads', value: 6500, targetValue: 10000, color: '#3b82f6' },
        { label: 'Meta Ads', value: 3200, targetValue: 5000, color: '#f59e0b' }
      ]" />

      <GoalTreemap title="Faixa Etária" :data="[
        { label: '36-45', value: 3443, color: '#900c3f' },
        { label: '26-35', value: 3417, color: '#c75b7a' },
        { label: '19-25', value: 2403, color: '#00a8cc' },
        { label: '0-18', value: 1033, color: '#00d2ff' },
        { label: '46-55', value: 1026, color: '#f90093' }
      ]" />

      <div style="display: flex; gap: 16px; margin-bottom: 32px;">
        <GoalKpiCard title="Total de Usuários" value="2,500" :percentage-diff="4" diff-period="vs. semana passada" />

        <GoalKpiCard title="Tempo Médio" value="123.5m" :percentage-diff="3" diff-period="vs. semana passada" />

        <GoalKpiCard title="Cancelamentos" value="43" :percentage-diff="-12" diff-period="vs. semana passada" />
      </div>
    </div>
    <GoalSplineArea title="Network Activities"
      :categories="['Jan 01', 'Jan 02', 'Jan 03', 'Jan 04', 'Jan 05', 'Jan 06']" :series="[
        { name: 'Facebook Campaign', data: [10, 70, 15, 38, 20, 80], color: '#6ee7b7' },
        { name: 'Twitter Campaign', data: [80, 20, 65, 10, 120, 10], color: '#4b9196' }
      ]" />
  </div>
</template>

<style scoped>
.dashboard {
  background-color: #0f172a;
  min-height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 50px;
  position: absolute;
  top: 0;
  left: 0;
}

.dash-header h1 {
  color: #f8fafc;
  font-family: sans-serif;
  margin-bottom: 40px;
}

.widgets-grid {
  display: flex;
  gap: 30px;
  flex-wrap: wrap;
  justify-content: center;
  max-width: 1000px;
}
</style>