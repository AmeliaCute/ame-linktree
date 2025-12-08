<template>
  <section class="hero">
    <div class="hero-content">
      <div class="profile-section">
        <ProfilePicture 
          :imageUrl="profile.image.url"
          :altText="profile.image.alt"
        />
        <ProfileInfo 
          :name="profile.name"
          :pronouns="profile.pronouns"
          :title="profile.title"
          :bio="profile.bio"
          :pronounsFlag="profile.pronounsFlag"
        />
        <SocialLinks :socialLinks="social" />
      </div>
    </div>
  </section>
</template>

<style scoped>
  .hero 
  {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
  }

  .hero-content 
  {
    max-width: 1200px;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 3rem;
  }

  .profile-section 
  {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;
    text-align: center;
  }

  @media (max-width: 768px) 
  {
    .hero-content 
    {
      padding: 0 4rem;
    }
  }
</style>

<script>
import ProfilePicture from './ProfilePicture.vue'
import ProfileInfo from './ProfileInfo.vue'
import SocialLinks from './SocialLinks.vue'

export default {
  name: 'HeroSection',
  components: {
    ProfilePicture,
    ProfileInfo,
    SocialLinks
  },
  props: {
    profile: {
      type: Object,
      required: true
    },
    social: {
      type: Array,
      required: true
    }
  }
}
</script>