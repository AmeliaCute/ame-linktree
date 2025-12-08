
<template>
  <div id="app" v-if="config">
    <BackgroundEffects 
      :tilt="tilt" 
      :backgroundImage="config.background.image"
    />

    <div class="content">
      <HeroSection 
        :profile="config.profile"
        :social="config.social"
      />
      <ProjectsSection 
        :projects="filteredProjects" 
      />
    </div>
  </div>
  <div v-else class="loading">
    <p>Loading...</p>
  </div>
</template>

<style>
  .content 
  {
    position: relative;
    z-index: 10;
  }

  .loading {
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    font-size: 1.5rem;
    color: #fff;
  }
</style>

<script>
import BackgroundEffects from './components/BackgroundEffects.vue'
import HeroSection from './components/HeroSection.vue'
import ProjectsSection from './components/ProjectsSection.vue'

export default {
  name: 'App',
  components: {
    BackgroundEffects,
    HeroSection,
    ProjectsSection,
  },
  data() {
    return {
      tilt: { x: 0, y: 0 },
      hasGyroscope: false,
      config: null
    }
  },
  computed: {
    filteredProjects() {
      if (!this.config) return []
      const featured = this.config.projects.filter(p => p.featured)
      return featured.length > 0 ? featured : this.config.projects
    }
  },
  async mounted() {
    await this.loadConfig()
    this.initGyroscope()
    this.initMouseMove()
    this.applyTheme()
  },
  methods: {
    async loadConfig() {
      try {
        const response = await fetch('/config.json')
        const text = await response.text()
        
        this.config = JSON.parse(text)
        
        console.log('Config loaded:', this.config)
      } catch (error) {
        console.error('Failed to load config:', error)
        throw error
      }
    },
    applyTheme() {
      if (!this.config || !this.config.theme) return
      
      const root = document.documentElement
      const colors = this.config.theme.colors
      
      root.style.setProperty('--color-primary', colors.primary)
      root.style.setProperty('--color-secondary', colors.secondary)
      root.style.setProperty('--color-accent', colors.accent)
    },
    initGyroscope() {
      const handleOrientation = (e) => {
        if (e.beta !== null && e.gamma !== null) {
          this.hasGyroscope = true
          const x = Math.max(-30, Math.min(30, e.gamma / 1.5))
          const y = Math.max(-30, Math.min(30, e.beta / 3))
          this.tilt = { x, y }
        }
      }

      if (typeof DeviceOrientationEvent !== 'undefined' && typeof DeviceOrientationEvent.requestPermission === 'function') {
        DeviceOrientationEvent.requestPermission()
          .then(permission => {
            if (permission === 'granted') {
              window.addEventListener('deviceorientation', handleOrientation)
            }
          })
      } else {
        window.addEventListener('deviceorientation', handleOrientation)
      }
    },
    initMouseMove() {
      window.addEventListener('mousemove', (e) => {
        if (!this.hasGyroscope) {
          const x = (e.clientX / window.innerWidth - 0.5) * 50
          const y = (e.clientY / window.innerHeight - 0.5) * 50
          this.tilt = { x, y }
        }
      })
    }
  }
}
</script>