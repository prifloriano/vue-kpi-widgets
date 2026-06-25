# Vue KPI Widgets 

Uma coleção moderna, reativa e minimalista de widgets e gráficos de performance (KPIs) construída com **Vue 3**, **TypeScript** e **Vite**. Ideal para dashboards analíticos e acompanhamento de métricas de negócio.

![Vue 3](https://img.shields.io/badge/Vue%203-35495E?style=for-the-badge&logo=vue.js&logoColor=4FC08D)
![TypeScript](https://img.shields.io/badge/typescript-%23007ACC.svg?style=for-the-badge&logo=typescript&logoColor=white)
![NPM Version](https://img.shields.io/npm/v/%40prifloriano%2fvue-kpi-widgets?style=for-the-badge&color=%23cb3837)
![License](https://img.shields.io/npm/l/%40prifloriano%2fvue-kpi-widgets?style=for-the-badge)

---

## Instalação

Instale o pacote diretamente através do NPM oficial:

    npm install @prifloriano/vue-kpi-widgets

---

## Como Usar

Importe os componentes desejados e o arquivo de estilos globais no seu `App.vue` ou componente de dashboard:

    <script setup lang="ts">
    import { 
      GoalKpiCard, 
      GoalSplineArea, 
      GoalTreemap 
    } from '@prifloriano/vue-kpi-widgets';

    // Importação obrigatória dos estilos
    import '@prifloriano/vue-kpi-widgets/dist/vue-kpi-widgets.css';
    </script>

    <template>
      <div class="dashboard">
        <!-- Card de Métrica Simples -->
        <GoalKpiCard :percentage-diff="15" diff-period="vs. mês passado" title="Faturamento Mensal" value="R$ 150.000"/>

        <!-- Gráfico de Área com Ondas -->
        <GoalSplineArea :categories="['Semana 1', 'Semana 2', 'Semana 3', 'Semana 4']" :series="[
            { name: 'Google Ads', data: [120, 240, 180, 320], color: '#3b82f6' },
            { name: 'Orgânico', data: [80, 90, 110, 140], color: '#10b981' }
          ]" title="Tráfego por Canal"/>
      </div>
    </template>

---

## Componentes Disponíveis

### `<GoalKpiCard />`
Card clássico para exibição de indicadores principais, suportando variação percentual positiva (verde) ou negativa (vermelha).

| Prop | Tipo | Obrigatório | Padrão | Descrição |
| :--- | :--- | :---: | :---: | :--- |
| `title` | `string` | Sim | - | Título do indicador |
| `value` | `string` | Sim | - | Valor principal formatado |
| `percentageDiff` | `number`| Não | `0` | Porcentagem de variação |
| `diffPeriod` | `string` | Não | `""` | Texto explicativo do período |

### `<GoalSplineArea />`
Gráfico de linhas suaves com preenchimento em degradê reativo.

### `<GoalTreemap />`
Gráfico de blocos proporcionais para análise de distribuição e share de métricas.

---

## 💻 Desenvolvimento Local

Abra o repositório, instale as dependências e execute o ambiente de visualização:

    # Clone o repositório
    git clone https://github.com/prifloriano/vue-kpi-widgets.git

    # Instale as dependências
    npm install

    # Rode o servidor de desenvolvimento
    npm run dev

---

Desenvolvido por **Priscila Floriano** — **Brasil**