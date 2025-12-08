<template>
  <div class="profile-wrapper">
    <div class="profile-glow"></div>
    <div class="profile-pic">
      <img :src="imageUrl" :alt="altText">
    </div>
  </div>
</template>

<style scoped>
  .profile-section 
  {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;
    text-align: center;
  }

  .profile-wrapper 
  {
    position: relative;
    animation: float 3s ease-in-out infinite;
  }

  @keyframes float 
  {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-10px); }
  }

  .profile-glow 
  {
    position: absolute;
    inset: -20px;
    background: linear-gradient(135deg, var(--color-primary), var(--color-secondary),  var(--color-accent));
    border-radius: 50%;
    filter: blur(30px);
    opacity: 0.6;
    animation: rotate 4s linear infinite;
  }

  @keyframes rotate 
  {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
  }

  .profile-pic 
  {
    position: relative;
    width: 200px;
    height: 200px;
    border-radius: 50%;
    overflow: hidden;
    border: 4px solid rgba(255, 255, 255, 0.3);
    background: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(20px);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4);
  }

  .profile-pic img 
  {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
</style>

<script>
export default {
  name: 'ProfilePicture',
  props: {
    imageUrl: {
      type: String,
      default: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Felix'
    },
    altText: {
      type: String,
      default: 'Profile'
    }
  }
}
</script>