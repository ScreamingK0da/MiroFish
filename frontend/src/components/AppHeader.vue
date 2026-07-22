<template>
  <header class="app-header">
    <div class="header-left">
      <div class="brand" @click="router.push('/')">MIROFISH</div>
    </div>

    <div class="header-center">
      <div class="view-switcher">
        <button
          v-for="mode in ['graph', 'split', 'workbench']"
          :key="mode"
          class="switch-btn"
          :class="{ active: viewMode === mode }"
          @click="$emit('update:viewMode', mode)"
        >
          {{ { graph: $t('main.layoutGraph'), split: $t('main.layoutSplit'), workbench: $t('main.layoutWorkbench') }[mode] }}
        </button>
      </div>
    </div>

    <div class="header-right">
      <LanguageSwitcher />
      <div class="step-divider"></div>
      <div class="workflow-step">
        <span class="step-num">Step {{ currentStep }}/5</span>
        <span class="step-name">{{ stepName }}</span>
      </div>
      <div class="step-divider"></div>
      <span class="status-indicator" :class="statusClass">
        <span class="dot"></span>
        {{ statusText }}
      </span>
    </div>
  </header>
</template>

<script setup>
import { useRouter } from 'vue-router'
import LanguageSwitcher from './LanguageSwitcher.vue'

const router = useRouter()

// Props
defineProps({
  viewMode: {
    type: String,
    required: true
  },
  currentStep: {
    type: Number,
    required: true
  },
  stepName: {
    type: String,
    required: true
  },
  statusClass: {
    type: String,
    required: true
  },
  statusText: {
    type: String,
    required: true
  }
})

// Emits
defineEmits(['update:viewMode'])
</script>

<style scoped>
@import '../assets/common-header.css';

.app-header {
  height: 60px;
  border-bottom: 1px solid #EAEAEA;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 24px;
  background: #FFF;
  z-index: 100;
  position: relative;
}

.header-center {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}

.brand {
  font-family: 'JetBrains Mono', monospace;
  font-weight: 800;
  font-size: 18px;
  letter-spacing: 1px;
  cursor: pointer;
}

.status-indicator {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 12px;
  color: #666;
  font-weight: 500;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #CCC;
}

.status-indicator.processing .dot { background: #FF5722; animation: pulse 1s infinite; }
.status-indicator.completed .dot { background: #4CAF50; }
.status-indicator.ready .dot { background: #4CAF50; }
.status-indicator.error .dot { background: #F44336; }

@keyframes pulse { 50% { opacity: 0.5; } }
</style>
