<template>
  <section class="projects-section">
    <div class="section-header">
      <h2 class="section-title">Featured Projects</h2>
      <div class="section-divider"></div>
    </div>

    <div class="projects-grid">
      <ProjectCard 
        v-for="project in projects" 
        :key="project.name"
        :project="project"
      />
    </div>
  </section>
</template>

<style scoped>
  .projects-section 
  {
    padding: 4rem 2rem;
    max-width: 1400px;
    margin: 0 auto;
  }

  .section-header 
  {
    text-align: center;
    margin-bottom: 3rem;
  }

  .section-title 
  {
    font-size: 3.5rem;
    font-weight: 900;
    background: linear-gradient(135deg, var(--color-primary), var(--color-secondary));
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 1rem;
  }

  .section-divider 
  {
    height: 4px;
    width: 120px;
    background: linear-gradient(90deg, var(--color-primary), var(--color-secondary));
    margin: 0 auto;
    border-radius: 2px;
  }

  .projects-grid 
  {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
    gap: 2rem;
  }

  @media (max-width: 768px) 
  {
    .section-title { font-size: 2.5rem; }
    .projects-grid { grid-template-columns: 1fr; }
  }
</style>

<script>
import ProjectCard from './ProjectCard.vue'

export default {
  name: 'ProjectsSection',
  components: {
    ProjectCard
  },
  props: {
    projects: {
      type: Array,
      required: true
    }
  }
}
</script>