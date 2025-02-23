\`\`\`vue type="vue" project="Enhanced Scrolling Text with Modern Dialog" file="App.vue"
<template>
  <div class="container">
    <div class="settings-icon" @click="showSettings = true">
      <Settings :size="24" color="#fff" />
    </div>
    <div class="marquee">
      <span class="marquee-content" :style="marqueeStyle">{{ displayText }}</span>
    </div>
  </div>

  <Teleport to="body">
    <div v-if="showSettings" class="dialog-overlay" @click="showSettings = false">
      <div class="dialog" @click.stop>
        <div class="dialog-header">
          <h2>Animation Settings</h2>
          <button class="close-button" @click="showSettings = false">
            <X :size="20" />
          </button>
        </div>
        
        <div class="dialog-content">
          <div class="setting-group">
            <label for="text">Display Text*</label>
            <input 
              id="text" 
              v-model="config.text" 
              placeholder="What text would you like to display?"
              class="modern-input"
            />
          </div>

          <div class="setting-group">
            <label for="fontSize">Font Size* ({{ config.fontSize }}px)</label>
            <div class="range-wrapper">
              <input 
                id="fontSize" 
                v-model="config.fontSize" 
                type="range" 
                min="30" 
                max="300" 
                step="1"
              />
              <div class="range-labels">
                <span>30px</span>
                <span>300px</span>
              </div>
            </div>
          </div>

          <div class="setting-group">
            <label for="speed">Animation Speed* ({{ config.speed }}s)</label>
            <div class="range-wrapper">
              <input 
                id="speed" 
                v-model="config.speed" 
                type="range" 
                min="5" 
                max="30" 
                step="1"
              />
              <div class="range-labels">
                <span>Fast</span>
                <span>Slow</span>
              </div>
            </div>
          </div>

          <div class="setting-group">
            <label>Gradient Style*</label>
            <div class="gradient-options">
              <div
                v-for="(gradient, index) in gradients"
                :key="index"
                class="gradient-option"
                :style="{ background: gradient }"
                :class="{ active: index === config.gradientIndex }"
                @click="selectGradient(index)"
              ></div>
            </div>
          </div>
        </div>

        <div class="dialog-footer">
          <button class="secondary" @click="showSettings = false">Cancel</button>
          <button class="primary" @click="saveSettings">Save changes</button>
        </div>
      </div>
    </div>
  </Teleport>
</template>

<script setup>
import { ref, reactive, computed } from 'vue';
import { Settings, X, Type } from 'lucide-vue-next';

const showSettings = ref(false);
const config = reactive({
  text: 'Sistema de letreiro digital feito para a Yelooo, realize os ajustes nas configurações',
  fontSize: 50,
  speed: 20,
  gradientIndex: 0
});

const gradients = [
  'linear-gradient(to right, #FF0080, #7928CA)',
  'linear-gradient(to right, #00DC82, #36E4DA)',
  'linear-gradient(to right, #FF4D4D, #F9CB28)',
  'linear-gradient(to right, #0070F3, #00DFD8)',
  'linear-gradient(to right, #7928CA, #FF0080)',
  'linear-gradient(to right, #FF8A00, #FF0080)',
  'linear-gradient(to right, #614385, #516395)',
  'linear-gradient(to right, #eecda3, #ef629f)',
  'linear-gradient(to right, #00b09b, #96c93d)',
  'linear-gradient(to right, #ff416c, #ff4b2b)'
];

const displayText = computed(() => config.text);

const marqueeStyle = computed(() => ({
  fontSize: `${config.fontSize}px`,
  background: gradients[config.gradientIndex],
  '-webkit-background-clip': 'text',
  '-webkit-text-fill-color': 'transparent',
  'background-clip': 'text',
  'animation': `marquee ${config.speed}s linear infinite`
}));

function selectGradient(index) {
  config.gradientIndex = index;
}

function saveSettings() {
  showSettings.value = false;
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap');

body, html {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background-color: black;
}

.container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  overflow: hidden;
}

.settings-icon {
  position: fixed;
  top: 20px;
  right: 20px;
  cursor: pointer;
  z-index: 10;
  background: rgba(255, 255, 255, 0.1);
  padding: 10px;
  border-radius: 50%;
  transition: background 0.3s ease;
}

.settings-icon:hover {
  background: rgba(255, 255, 255, 0.2);
}

.marquee {
  position: relative;
  width: 100%;
  overflow: hidden;
}

.marquee-content {
  display: inline-block;
  padding-left: 100%;
  white-space: nowrap;
  animation: marquee var(--duration, 20s) linear infinite;
  will-change: transform;
}

/* Updated input styling to match the reference */
.modern-input {
  width: 100%;
  padding: 16px;
  font-size: 1rem;
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  background-color: white;
  color: #1a202c;
  transition: all 0.2s ease;
  box-sizing: border-box;
  outline: none;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
}

.modern-input:hover {
  border-color: #cbd5e0;
}

.modern-input:focus {
  border-color: #7928CA;
  box-shadow: 0 0 0 1px #7928CA;
}

.modern-input::placeholder {
  color: #a0aec0;
}

/* Remove the input-wrapper and icon styles since we're using a cleaner design */
.input-wrapper {
  width: 100%;
}

@keyframes marquee {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(-100%);
  }
}

.dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 20;
}

.dialog {
  background: white;
  border-radius: 12px;
  width: 500px;
  color: #1a1a1a;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

.dialog-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 24px 24px 0;
}

.dialog-header h2 {
  margin: 0;
  font-size: 1.25rem;
  font-weight: 600;
  color: #1a1a1a;
}

.close-button {
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  color: #6b7280;
  border-radius: 6px;
}

.close-button:hover {
  background: #f3f4f6;
}

.dialog-content {
  padding: 24px;
}

.setting-group {
  margin-bottom: 24px;
}

.setting-group:last-child {
  margin-bottom: 0;
}

label {
  display: block;
  margin-bottom: 8px;
  font-size: 0.875rem;
  font-weight: 500;
  color: #374151;
}

.range-wrapper {
  padding: 0 4px;
}

input[type="range"] {
  width: 100%;
  margin: 8px 0;
}

.range-labels {
  display: flex;
  justify-content: space-between;
  font-size: 0.75rem;
  color: #6b7280;
}

.gradient-options {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 12px;
  padding: 4px;
}

.gradient-option {
  aspect-ratio: 1;
  border-radius: 8px;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
}

.gradient-option:hover {
  transform: scale(1.05);
}

.gradient-option.active {
  box-shadow: 0 0 0 2px white, 0 0 0 4px #7928CA;
}

.dialog-footer {
  padding: 24px;
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  border-top: 1px solid #e5e7eb;
}

button.primary,
button.secondary {
  padding: 8px 16px;
  border-radius: 6px;
  font-size: 0.875rem;
  font-weight: 500;
  transition: all 0.2s;
}

button.primary {
  background: #7928CA;
  color: white;
  border: none;
}

button.primary:hover {
  background: #6821ab;
}

button.secondary {
  background: white;
  color: #374151;
  border: 1px solid #e5e7eb;
}

button.secondary:hover {
  background: #f3f4f6;
}
</style>