<template>
  <div class="postcard">

    <div class="pc-wrapper">
       <div class="pc-content">
        <div class="pc-to-section">
          <div class="pc-to-line">
            <span class="pc-label">Pr:</span>
            <span class="pc-recipient">{{ project.name }}</span>
          </div>
          <div class="pc-underline"></div>
        </div>

        <div style="display: flex; height: 100%; justify-content: space-between; flex-direction: column;">
          <div class="pc-body">
            <p class="pc-desc">
              {{ project.desc }}
              <span class="pc-highlight" v-if="project.highlight">{{ project.highlight }}</span>
            </p>
          </div>
          
          <div class="pc-links">
            <a :href="project.github" target="_blank" class="pc-link-text">View Code</a>
            <span v-if="project.live">/</span>
            <a v-if="project.live" :href="project.live.url" target="_blank" class="pc-link-text">
              {{ project.live.label }}
            </a>
          </div>
        </div>
      </div>

      <div class="pc-foot">
        <span class="pc-postcard-lbl">SayGex~</span>
        <div class="pc-logo-icon"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  project: { type: Object, required: true },
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Nunito:wght@400;700;900&family=Caveat&display=swap');

.postcard 
{
  position: relative;
  width: 100%;
  aspect-ratio: 1.6 / 1;
  background: #fdf1ec;
  border-radius: .25rem;
  box-shadow: 0 1.25rem 3.125rem rgba(0,0,0,0.3);
  padding: 1.25rem;
  box-sizing: border-box;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.pc-wrapper
{
  border: .1875rem solid #D9BDB2; 
  padding: .75rem; 
  border-radius: .25rem; 
  display: flex; 
  flex-direction: column;
}

.pc-content 
{
  flex: 1;
  display: flex;
  flex-direction: column;
}

.pc-to-section
{
  margin-bottom: 1.875rem;
}

.pc-to-line 
{
  display: flex;
  align-items: baseline;
  gap: .625rem;
}

.pc-label 
{
  font-family: celeste;
  font-weight: 900;
  font-size: 1.8rem;
  color: #D09E92;
}

.pc-recipient 
{
  font-family: 'Nunito', sans-serif;
  font-weight: 700;
  font-size: 1.8rem;
  color: #4b4b4b;
}

.pc-underline 
{
  height: .125rem;
  background: #d4c1b3;
  width: 40%;
  margin-top: .25rem;
  box-shadow: .25rem .375rem 0 #d4c1b3;
}

.pc-body 
{
  
  max-width: 85%;
}

.pc-desc 
{
  font-family: 'Caveat', cursive;
  font-weight: 700;
  font-size: 1.25rem;
  line-height: 1.4;
  color: #4b4b4b;
}

.pc-highlight 
{
  color: #ff2d4b;
  font-weight: 900;
}

.pc-links 
{
  margin-bottom: .9375rem;
  display: flex;
  gap: .625rem;
  font-family: 'Nunito', sans-serif;
  font-weight: 800;
  font-size: 0.9rem;
  color: #d4c1b3;
}

.pc-link-text 
{
  text-decoration: none;
  color: #4b4b4b;
  border-bottom: .0625rem solid transparent;
}

.pc-link-text:hover 
{
  border-bottom: .0625rem solid #4b4b4b;
}

.pc-foot 
{
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
}

.pc-postcard-lbl 
{
  font-family: 'Caveat', cursive;
  font-size: 1.2rem;
  color: #bfa899;
}

.pc-logo-icon 
{
  width: .75rem;
  height: .75rem;
  background: #bfa899;
  clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
}

@media (width > 648px) 
{
  .pc-stamp-inner
  {
    height: 100%;
  }

  .pc-wrapper
  {
    height: 100%;
  }

  .postcard
  {
    max-width: 600px;
  }  
}
</style>