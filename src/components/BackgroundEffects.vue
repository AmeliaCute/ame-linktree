<template>
  <div>
    <div class="background" :style="backgroundStyle"></div>
    <div class="acrylic-noise"></div>
    <div class="gradient-overlay"></div>
  </div>
</template>

<style scoped>
  .background 
  {
    position: fixed;
    inset: 0;
    background-size: cover;
    background-position: center;
    filter: blur(15px) brightness(0.35);
    transition: transform 0.5s ease-out;
    z-index: 1;
  }

  .acrylic-noise 
  {
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 2;
    opacity: 0.2;
    mix-blend-mode: overlay;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 100 100' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='4' numOctaves='8' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
  }

  .gradient-overlay 
  {
    position: fixed;
    inset: 0;
    background: linear-gradient(135deg, rgba(255, 102, 196, 0.08) 0%, rgba(102, 126, 234, 0.08) 100%);
    z-index: 2;
  }
</style>

<script>
export default {
  name: 'BackgroundEffects',
  props: {
    tilt: {
      type: Object,
      required: true
    },
    backgroundImage: {
      type: String
    }
  },
  computed: {
    backgroundStyle() {
      return {
        backgroundImage: `url(${this.backgroundImage})`,
        transform: `translate(${this.tilt.x}px, ${this.tilt.y}px) scale(1.15)`
      }
    }
  }
}
</script>