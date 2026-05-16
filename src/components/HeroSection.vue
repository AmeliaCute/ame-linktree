<template>
  <section class="hero">
    <div class="hero-inner">

      <div class="av-float">
        <div class="av-img">
          <img :src="PROFILE.avatar" :alt="PROFILE.name" />
        </div>
      </div>

      <div class="hero-handle">{{ PROFILE.handle }}</div>
      <h1 class="hero-name">{{ PROFILE.name }}</h1>
      <p class="hero-title">{{ PROFILE.title }}</p>
      <div class="pronouns"><span>{{ PROFILE.pronouns }}</span></div>

      <nav class="hero-links">
        <SocialLink
          v-for="s in SOCIALS"
          :key="s.label"
          :social="s"
        />
      </nav>
    </div>
  </section>
</template>

<script setup>
import { PROFILE, SOCIALS } from '../config.js'
import SocialLink from './SocialLink.vue'
</script>

<style scoped>
.hero 
{
  position: relative;
  z-index: 10;
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding: 0 7vw;
  margin-top: 3.5rem;
}

.hero-inner 
{
  display: flex;
  flex-direction: column;
}


.av-float 
{
  position: relative;
  width: 12rem; height: 12rem;
  margin-bottom: 1.25rem;
  animation: float 3.8s ease-in-out infinite;
}

.av-ring
 {
  position: absolute;
  inset: -0.375rem;
  border-radius: 50%;
  background: conic-gradient(from 0deg, #ff99cc, #66aaff, #aaddff, #ff99cc);
  filter: blur(.625rem);
  opacity: .65;
  animation: spin 5s linear infinite;
}

.av-img 
{
  position: relative;
  width: 100%; height: 100%;
  border-radius: 50%;
  overflow: hidden;
  border: .1875rem solid rgba(255,255,255,.45);
  box-shadow: 0 .375rem 1.5rem rgba(0,0,0,.5);
}

.av-img img 
{ 
  width: 100%; 
  height: 100%; 
  object-fit: cover; 
}

.hero-handle 
{
  font-family: celeste;
  font-size: .72rem;
  letter-spacing: .4em;
  color: rgba(180,210,255,.7);
  text-transform: uppercase;
  text-shadow: 0 0 1rem rgba(100,160,255,.6);
  margin-bottom: .375rem;
}

.hero-name 
{
  font-family: celeste;
  font-size: clamp(3rem, 8vw, 7rem);
  color: #fff;
  line-height: 1;
  letter-spacing: .03em;
  text-shadow:
    .25rem .25rem 0 rgba(40,70,160,.6),
    .5rem .5rem 0 rgba(20,40,100,.4),
    0 0 2.5rem rgba(100,160,255,.35);
  margin-bottom: .625rem;
}

.hero-title 
{
  font-weight: 700;
  font-size: clamp(.9rem, 2vw, 1.15rem);
  color: rgba(180,210,255,.75);
  margin-bottom: 1.75rem;
}

.pronouns 
{
  display: inline-flex;
  margin-bottom: 2rem;
}

.pronouns span 
{
  padding: .1875rem .75rem;
  border-radius: .25rem;
  font-size: .85rem;
  font-weight: 800;
  color: rgba(220,235,255,.8);
  background: rgba(255,255,255,.08);
  border: .0938rem solid rgba(255,255,255,.18);
}

.hero-links 
{
  display: flex;
  flex-direction: column;
  gap: .25rem;
}

@media (width <= 648px) 
{
  .hero-name
  {
    text-shadow:
    .15rem .15rem 0 rgba(40,70,160,.6),
    .3rem .3rem 0 rgba(20,40,100,.4),
    0 0 2.5rem rgba(100,160,255,.35);
  }  
}
</style>
