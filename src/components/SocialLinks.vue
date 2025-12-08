<template>
  <div class="social-links">
    <a 
      v-for="link in socialLinks" 
      :key="link.name"
      :href="link.url" 
      target="_blank" 
      class="social-btn"
    >
      <div class="icon-container">
        <component :is="getIcon(link.icon)" class="icon" />
        {{ link.name }}
      </div>
    </a>
  </div>
</template>

<style scoped>
  .social-links 
  {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 2rem;
  }

  .social-btn 
  {
    position: relative;
    padding: 1rem 2rem;
    border-radius: 50px;
    font-weight: 800;
    font-size: 1rem;
    background: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(20px);
    border: 3px solid rgba(255, 255, 255, 0.15);
    color: #fff;
    text-decoration: none;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    transition: all 0.3s ease;
    overflow: hidden;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  }

  .social-btn:hover 
  {
    transform: translateY(-3px) scale(1.05);
    border-color: rgba(255, 255, 255, 0.4);
    box-shadow: 0 12px 40px rgba(255, 102, 196, 0.4);
  }

  .social-btn::before 
  {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(255, 102, 196, 0.3), rgba(102, 126, 234, 0.3));
    opacity: 0;
    transition: opacity 0.3s ease;
  }

  .social-btn:hover::before 
  {
    opacity: 1;
  }

  .social-btn span 
  {
    position: relative;
    z-index: 1;
  }

  .icon-container 
  {
    display: flex;
    gap: 0.5rem;
    align-items: center;
    overflow: hidden;
    position: relative;
    z-index: 1;
  }

  
</style>

<script>
import GithubIcon from './icons/GithubIcon.vue'
import EmailIcon from './icons/EmailIcon.vue'
import TwitchIcon from './icons/TwitchIcon.vue'
import TiktokIcon from './icons/TiktokIcon.vue'
import DiscordIcon from './icons/DiscordIcon.vue'
import YouTubeIcon from './icons/YouTubeIcon.vue'

export default {
  name: 'SocialLinks',
  components: {
    GithubIcon,
    EmailIcon,
    TwitchIcon,
    TiktokIcon,
    DiscordIcon,
    YouTubeIcon
  },
  props: {
    socialLinks: {
      type: Array,
      default: () => [
        {
          name: 'GitHub',
          url: 'https://github.com/yourusername',
          icon: 'github'
        },
        {
          name: 'Email',
          url: 'mailto:your.email@example.com',
          icon: 'email'
        }
      ]
    }
  },
  methods: {
    getIcon(iconName) {
      const icons = {
        github: 'GitHubIcon',
        email: 'EmailIcon',
        twitch: 'TwitchIcon',
        tiktok: 'TikTokIcon',
        discord: 'DiscordIcon',
        youtube: 'YouTubeIcon'
      }
      return icons[iconName] || 'GitHubIcon'
    }
  }
}
</script>