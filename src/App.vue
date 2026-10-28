<script setup lang="ts">
import { ref } from 'vue'

type Zone = {
  name: string
  status: string
  metric: string
  icon: string
  tone: 'ok' | 'warning' | 'alert'
}

const modes = ['Rainfall Watch', 'Crop Advisory', 'Market Pulse', 'Field Survey']
const currentMode = ref('Rainfall Watch')
const monitoringActive = ref(true)

const summary = [
  { label: 'Active farms', value: '156', detail: '+18 this month' },
  { label: 'Irrigation coverage', value: '89%', detail: 'Across 9 barangays' },
  { label: 'Soil health', value: '76%', detail: 'Stable conditions' },
  { label: 'Response time', value: '2.8h', detail: 'Fast field updates' }
]

const zones: Zone[] = [
  { name: 'Upper Canlaon', status: 'Healthy crop growth', metric: 'Optimal', icon: '🌾', tone: 'ok' },
  { name: 'Rice Fields', status: 'Moisture level normal', metric: '12 mm', icon: '💧', tone: 'ok' },
  { name: 'Vegetable Belt', status: 'Mild stress alert', metric: '2 farms', icon: '🥬', tone: 'warning' },
  { name: 'Cattle Pasture', status: 'Water demand rising', metric: 'High', icon: '🐄', tone: 'alert' }
]

const alerts = [
  { level: 'Critical', title: 'Storm risk in upland farms', time: 'Just now', location: 'Barangay Masulog' },
  { level: 'Warning', title: 'Low soil moisture detected', time: '18 min ago', location: 'Sitio Kalbas' },
  { level: 'Info', title: 'Farmer advisory bulletin issued', time: '1 hr ago', location: 'City monitoring desk' }
]

const devices = [
  { name: 'Weather Station', state: 'Online', power: '98%' },
  { name: 'Soil Sensor', state: 'Monitoring', power: '94%' },
  { name: 'Water Pump', state: 'Running', power: '82%' },
  { name: 'Drone Feed', state: 'Connected', power: '90%' }
]

const toggleSystem = () => {
  monitoringActive.value = !monitoringActive.value
  currentMode.value = monitoringActive.value ? 'Rainfall Watch' : 'Standby Mode'
}
</script>

<template>
  <div class="page-shell">
    <header class="topbar">
      <div class="brand-wrap">
        <div class="brand-mark">A</div>
        <div>
          <p class="eyebrow">Department of Agriculture</p>
          <h2>AgriSight</h2>
        </div>
      </div>

      <nav class="nav">
        <a href="#overview">Overview</a>
        <a href="#zones">Monitoring</a>
        <a href="#alerts">Alerts</a>
        <a href="#devices">Sensors</a>
      </nav>

      <button class="security-toggle" :class="{ armed: monitoringActive }" @click="toggleSystem">
        {{ monitoringActive ? 'Monitoring Active' : 'Monitoring Standby' }}
      </button>
    </header>

    <main class="dashboard">
      <section id="overview" class="hero panel">
        <div class="hero-copy">
          <p class="eyebrow accent">Canlaon City agriculture intelligence</p>
          <h1>Web-Based Real-Time Farmer Monitoring and Agricultural Information System for the Department of Agriculture in Canlaon City</h1>
          <p class="subtitle">
            A live digital monitoring platform designed to help farmers, agricultural technicians, and local authorities
            track field conditions, crop health, irrigation, weather, and advisory updates in real time.
          </p>

          <div class="cta-row">
            <button class="primary-btn">Send advisory</button>
            <button class="secondary-btn">Review reports</button>
          </div>
        </div>

        <div class="status-box">
          <div class="radar">
            <span class="core"></span>
            <span class="ring ring-1"></span>
            <span class="ring ring-2"></span>
            <span class="ring ring-3"></span>
          </div>

          <div class="status-head">
            <span>Status</span>
            <strong>{{ monitoringActive ? 'Operational' : 'Standby' }}</strong>
          </div>

          <div class="status-value">{{ monitoringActive ? '91.4%' : 'Safe Mode' }}</div>

          <div class="mini-metrics">
            <div>
              <small>Fields</small>
              <strong>12</strong>
            </div>
            <div>
              <small>Alerts</small>
              <strong>03</strong>
            </div>
            <div>
              <small>Latency</small>
              <strong>2.8h</strong>
            </div>
          </div>
        </div>
      </section>

      <section class="stats-grid">
        <article v-for="item in summary" :key="item.label" class="panel stat-card">
          <p>{{ item.label }}</p>
          <h3>{{ item.value }}</h3>
          <span>{{ item.detail }}</span>
        </article>
      </section>

      <section class="content-grid">
        <article id="zones" class="panel">
          <div class="panel-head">
            <div>
              <p class="eyebrow">Field coverage</p>
              <h3>Monitoring overview</h3>
            </div>

            <select v-model="currentMode">
              <option v-for="mode in modes" :key="mode" :value="mode">{{ mode }}</option>
            </select>
          </div>

          <div class="zone-list">
            <div v-for="zone in zones" :key="zone.name" class="zone-item">
              <div class="zone-icon" :class="zone.tone">{{ zone.icon }}</div>
              <div class="zone-copy">
                <h4>{{ zone.name }}</h4>
                <p>{{ zone.status }}</p>
              </div>
              <span class="zone-metric">{{ zone.metric }}</span>
            </div>
          </div>
        </article>

        <article id="alerts" class="panel alerts-panel">
          <div class="panel-head">
            <div>
              <p class="eyebrow">Live updates</p>
              <h3>Field alerts</h3>
            </div>
            <button class="link-btn">View all</button>
          </div>

          <ul class="alert-list">
            <li v-for="alert in alerts" :key="alert.title" class="alert-item">
              <span class="alert-badge" :class="alert.level.toLowerCase()">{{ alert.level }}</span>
              <div>
                <strong>{{ alert.title }}</strong>
                <p>{{ alert.location }} • {{ alert.time }}</p>
              </div>
            </li>
          </ul>
        </article>
      </section>

      <section id="devices" class="bottom-grid">
        <article class="panel camera-panel">
          <div class="panel-head">
            <div>
              <p class="eyebrow">Farm view</p>
              <h3>Field camera preview</h3>
            </div>
            <span class="live-dot">● Live</span>
          </div>

          <div class="camera-view">
            <div class="camera-overlay">
              <span>Barangay Rizal</span>
              <strong>Rice field status: healthy</strong>
            </div>
          </div>
        </article>

        <article class="panel devices-panel">
          <div class="panel-head">
            <div>
              <p class="eyebrow">Connected systems</p>
              <h3>Monitoring network</h3>
            </div>
          </div>

          <div class="device-list">
            <div v-for="device in devices" :key="device.name" class="device-item">
              <div>
                <strong>{{ device.name }}</strong>
                <p>{{ device.state }}</p>
              </div>
              <span>{{ device.power }}</span>
            </div>
          </div>
        </article>
      </section>
    </main>
  </div>
</template>
