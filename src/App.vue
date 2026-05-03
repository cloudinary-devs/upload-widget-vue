<template>
  <div class="page">

    <header class="header">
      <div class="header-inner">
        <div class="logo">Cloudinary Upload Widget</div>
        <span class="badge">Vue</span>
      </div>
    </header>

    <main class="main">

      <section class="hero">
        <h1>Upload files with<br><span class="highlight">Cloudinary</span></h1>
        <p class="subtitle">
          A minimal Vue integration using the Cloudinary Upload Widget.
          Click the button below to open the uploader.
        </p>

        <UploadWidget
          :cloudName="cloudName"
          :uploadPreset="uploadPreset"
          @uploaded="onUploaded"
        />

        <div class="config-info">
          <span class="config-item"><strong>Cloud:</strong> {{ cloudName }}</span>
          <span class="sep">·</span>
          <span class="config-item"><strong>Preset:</strong> {{ uploadPreset }}</span>
        </div>
      </section>

      <section v-if="uploads.length > 0" class="results">
        <h2>Uploaded Files <span class="count">{{ uploads.length }}</span></h2>
        <div class="grid">
          <div v-for="item in uploads" :key="item.public_id" class="card">
            <div class="card-thumb">
              <img :src="item.secure_url" :alt="item.public_id" loading="lazy" />
            </div>
            <div class="card-body">
              <p class="card-name">{{ item.public_id.split('/').pop() }}</p>
              <div class="card-meta">
                <span class="tag">{{ item.format.toUpperCase() }}</span>
                <span class="size">{{ formatBytes(item.bytes) }}</span>
                <span v-if="item.width" class="dims">{{ item.width }}×{{ item.height }}</span>
              </div>
              <a :href="item.secure_url" target="_blank" rel="noopener" class="view-link">
                View on Cloudinary ↗
              </a>
            </div>
          </div>
        </div>
      </section>

      <section v-else class="empty">
        <div class="empty-icon">
          <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <rect x="3" y="3" width="18" height="18" rx="2"/>
            <circle cx="8.5" cy="8.5" r="1.5"/>
            <polyline points="21 15 16 10 5 21"/>
          </svg>
        </div>
        <p>No uploads yet. Click <strong>Upload Files</strong> to get started.</p>
      </section>

    </main>

  </div>
</template>

<script setup>
import { ref } from 'vue'
import UploadWidget from './components/UploadWidget.vue'

const cloudName = 'hzxyensd5'
const uploadPreset = 'aoh4fpwm'

const uploads = ref([])

function onUploaded(info) {
  uploads.value = [info, ...uploads.value]
}

function formatBytes(bytes) {
  if (bytes < 1024) return `${bytes} B`
  if (bytes < 1024 * 1024) return `${(bytes / 1024).toFixed(1)} KB`
  return `${(bytes / (1024 * 1024)).toFixed(1)} MB`
}
</script>

<style>
*, *::before, *::after { box-sizing: border-box; }

body {
  margin: 0;
  padding: 0;
  background: #f8f9fc;
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
  -webkit-font-smoothing: antialiased;
}

/* Layout */
.page { min-height: 100vh; }

/* Header */
.header {
  background: #fff;
  border-bottom: 1px solid #e8ecf0;
  position: sticky;
  top: 0;
  z-index: 10;
}

.header-inner {
  max-width: 900px;
  margin: 0 auto;
  padding: 0 24px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.logo {
  font-weight: 600;
  font-size: 16px;
  color: #1a1a2e;
}

.badge {
  font-size: 12px;
  font-weight: 600;
  color: #41b883;
  background: #f0faf5;
  border: 1px solid #b2e6cf;
  padding: 2px 10px;
  border-radius: 20px;
  letter-spacing: 0.5px;
}

/* Main */
.main {
  max-width: 900px;
  margin: 0 auto;
  padding: 60px 24px 80px;
}

/* Hero */
.hero {
  text-align: center;
  margin-bottom: 64px;
}

.hero h1 {
  font-size: clamp(2rem, 5vw, 3rem);
  font-weight: 800;
  color: #1a1a2e;
  line-height: 1.15;
  margin: 0 0 16px;
  letter-spacing: -0.5px;
}

.highlight {
  background: linear-gradient(135deg, #41b883 0%, #35495e 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.subtitle {
  font-size: 17px;
  color: #5e6a7a;
  line-height: 1.6;
  max-width: 480px;
  margin: 0 auto 36px;
}

/* Upload Button */
.upload-btn {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  background: linear-gradient(135deg, #41b883 0%, #35495e 100%);
  color: #fff;
  border: none;
  border-radius: 12px;
  padding: 14px 32px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: opacity 0.2s, transform 0.15s;
  box-shadow: 0 4px 16px rgba(65, 184, 131, 0.35);
}

.upload-btn:hover {
  opacity: 0.9;
  transform: translateY(-1px);
}

.upload-btn:active {
  transform: translateY(0);
  opacity: 1;
}

/* Config info */
.config-info {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-top: 20px;
  font-size: 13px;
  color: #8a95a3;
}

.sep { color: #c9d0d8; }

/* Results */
.results h2 {
  font-size: 20px;
  font-weight: 700;
  color: #1a1a2e;
  margin: 0 0 24px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.count {
  font-size: 13px;
  font-weight: 600;
  background: #f0faf5;
  color: #41b883;
  padding: 2px 9px;
  border-radius: 20px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 20px;
}

/* Card */
.card {
  background: #fff;
  border: 1px solid #e8ecf0;
  border-radius: 14px;
  overflow: hidden;
  transition: box-shadow 0.2s, transform 0.2s;
}

.card:hover {
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
  transform: translateY(-2px);
}

.card-thumb {
  width: 100%;
  aspect-ratio: 16/9;
  background: #f0f2f8;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.card-thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-body { padding: 14px 16px 16px; }

.card-name {
  font-size: 14px;
  font-weight: 600;
  color: #1a1a2e;
  margin: 0 0 8px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.card-meta {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 12px;
  flex-wrap: wrap;
}

.tag {
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.5px;
  background: #f0faf5;
  color: #41b883;
  padding: 2px 7px;
  border-radius: 6px;
}

.size, .dims { font-size: 12px; color: #8a95a3; }

.view-link {
  font-size: 13px;
  font-weight: 500;
  color: #41b883;
  text-decoration: none;
}

.view-link:hover { text-decoration: underline; }

/* Empty state */
.empty {
  text-align: center;
  padding: 64px 0;
  color: #8a95a3;
}

.empty-icon {
  display: flex;
  justify-content: center;
  margin-bottom: 16px;
  opacity: 0.35;
}

.empty p { font-size: 15px; line-height: 1.6; margin: 0; }
</style>
