<template>
  <div>
    <h1 class="name">{{ name }}</h1>
    <div 
      class="pronouns" 
      :class="{ 'flag-active': flagConfig.enabled }"
      :style="pronounsStyle"
    >
      <span class="pronouns-text">{{ pronouns }}</span>
    </div>
    <h2 class="title">{{ title }}</h2>
    <p class="bio">{{ bio }}</p>
  </div>
</template>

<style scoped>
  .name 
  {
    font-size: 4rem;
    font-weight: 900;
    background: linear-gradient(135deg, var(--color-primary), var(--color-secondary),  var(--color-accent));
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    letter-spacing: -2px;
    margin-bottom: 0.5rem;
  }

  .pronouns 
  {
    position: relative;
    display: inline-block;
    padding: 0.5rem 1.5rem;
    background: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(20px);
    border: 3px solid rgba(255, 255, 255, 0.15);
    border-radius: 50px;
    font-size: 1rem;
    font-weight: 700;
    margin-bottom: 1rem;
    color: #fff;
    cursor: pointer;
    transition: border-color 0.3s ease;
  }

  .pronouns.flag-active:hover 
  {
    border-color: transparent;
  }

  .pronouns.flag-active:hover::before 
  {
    content: '';
    position: absolute;
    inset: -3px;
    border-radius: 50px;
    background: linear-gradient(to right, var(--flag-colors));
    -webkit-mask: 
      linear-gradient(#fff 0 0) content-box, 
      linear-gradient(#fff 0 0);
    -webkit-mask-composite: xor;
    mask: 
      linear-gradient(#fff 0 0) content-box, 
      linear-gradient(#fff 0 0);
    mask-composite: exclude;
    padding: 3px;
    pointer-events: none;
    z-index: -1;
  }

  .pronouns-text {
    position: relative;
    z-index: 2;
  }

  .title 
  {
    font-size: 2rem;
    font-weight: 800;
    color: #ddd;
    margin-bottom: 1rem;
  }

  .bio 
  {
    font-size: 1.1rem;
    color: #bbb;
    line-height: 1.6;
  }

  @media (max-width: 768px) 
  {
    .name { font-size: 2.5rem; }
    .title { font-size: 1.5rem; }
  }
</style>

<script>
export default 
{
  name: 'ProfileInfo',
  props: {
    name: {
      type: String,
      default: 'Your Name'
    },
    pronouns: {
      type: String,
      default: 'they/them'
    },
    title: {
      type: String,
      default: 'Full Stack Developer'
    },
    bio: {
      type: String,
      default: 'Building amazing things'
    },
    pronounsFlag: {
      type: Object,
      default: () => ({
        enabled: false,
        colors: []
      })
    }
  },
  computed: {
    flagConfig() {
      return {
        enabled: this.pronounsFlag?.enabled || false,
        colors: this.pronounsFlag?.colors || []
      }
    },
    pronounsStyle() 
    {
      if (!this.flagConfig.enabled || this.flagConfig.colors.length === 0) 
        return {}
      
      const colorStops = this.flagConfig.colors.map((color, index) => 
      {
        const startPercent = (index / this.flagConfig.colors.length) * 100
        const endPercent = ((index + 1) / this.flagConfig.colors.length) * 100
        return `${color} ${startPercent}%, ${color} ${endPercent}%`
      }).join(', ')
      
      return {
        '--flag-colors': colorStops
      }
    }
  }
}
</script>