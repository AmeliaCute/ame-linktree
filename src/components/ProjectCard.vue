<template>
  <div class="project-card">
    <div class="project-content">
      <div class="project-header">
        <h3 class="project-name">{{ project.name }}</h3>
        <div class="stars-badge">
          <StarIcon />
          {{ project.stars }}
        </div>
      </div>
      
      <p class="project-description">{{ project.description }}</p>
      
      <div class="project-tags">
        <span v-for="tag in project.tags" :key="tag" class="tag">{{ tag }}</span>
      </div>

      <div class="project-links">
        <a :href="project.github" target="_blank" class="project-btn secondary">
          <GitHubIcon />
          Code
        </a>
        <a v-if="project.live" :href="project.live" target="_blank" class="project-btn primary">
          <ExternalLinkIcon />
          Docs
        </a>
      </div>
    </div>
  </div>
</template>

<style>
  .project-card 
  {
    position: relative;
    padding: 2rem;
    border-radius: 20px;
    background: rgba(255, 255, 255, 0.03);
    backdrop-filter: blur(20px);
    border: 3px solid rgba(255, 255, 255, 0.1);
    transition: all 0.3s ease;
    overflow: hidden;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  }

  .project-card::before 
  {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(255, 102, 196, 0.1), rgba(102, 126, 234, 0.1));
    opacity: 0;
    transition: opacity 0.3s ease;
  }

  .project-card:hover 
  {
    transform: translateY(-5px) scale(1.02);
    border-color: rgba(255, 255, 255, 0.3);
    box-shadow: 0 12px 40px rgba(105, 102, 255, 0.3);
  }

  .project-card:hover::before 
  {
    opacity: 1;
  }

  .project-content 
  {
    position: relative;
    z-index: 1;
  }

  .project-header 
  {
    display: flex;
    justify-content: space-between;
    align-items: start;
    margin-bottom: 1rem;
  }

  .project-name 
  {
    font-size: 1.8rem;
    font-weight: 900;
    color: #fff;
  }

  .stars-badge 
  {
    display: flex;
    align-items: center;
    gap: 0.3rem;
    padding: 0.5rem 1rem;
    background: rgba(255, 215, 0, 0.15);
    border: 2px solid rgba(255, 215, 0, 0.3);
    border-radius: 50px;
    font-weight: 800;
    font-size: 0.9rem;
  }

  .project-description 
  {
    color: #ccc;
    margin-bottom: 1.5rem;
    line-height: 1.6;
  }

  .project-tags 
  {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-bottom: 1.5rem;
  }

  .tag 
  {
    padding: 0.5rem 1rem;
    background: rgba(255, 255, 255, 0.1);
    border: 2px solid rgba(255, 255, 255, 0.2);
    border-radius: 50px;
    font-size: 0.85rem;
    font-weight: 700;
  }

  .project-links 
  {
    display: flex;
    gap: 1rem;
  }

  .project-btn 
  {
    flex: 1;
    padding: 0.8rem 1.5rem;
    border-radius: 12px;
    font-weight: 800;
    text-decoration: none;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    transition: all 0.3s ease;
    border: 2px solid rgba(255, 255, 255, 0.2);
    color: #fff;
  }

  .project-btn.primary 
  {
    background: rgba(255, 255, 255, 0.05);
    border-color: rgba(133, 102, 255, 0.305);
  }

  .project-btn.secondary 
  {
    background: rgba(255, 255, 255, 0.05);
  }

  .project-btn:hover 
  {
    transform: translateY(-2px);
  }
</style>

<script>
import GitHubIcon from './icons/GithubIcon.vue'
import StarIcon from './icons/StarIcon.vue'
import ExternalLinkIcon from './icons/ExternalLinkIcon.vue'

export default {
  name: 'ProjectCard',
  components: {
    GitHubIcon,
    StarIcon,
    ExternalLinkIcon
  },
  props: {
    project: {
      type: Object,
      required: true
    }
  }
}
</script>