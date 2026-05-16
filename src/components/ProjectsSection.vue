<template>
  <div ref="track" class="track" :style="{ height: trackHeight + 'px' }">
    <div class="sticky">
      <div class="section-lbl">Projets</div>

      <div ref="stage" class="stage" :style="{ height: stageHeight + 'px' }">
        <PostCard
          v-for="(p, i) in PROJECTS"
          :key="p.name"
          :project="p"
          :ref="el => cardRefs[i] = el"
          class="card-slot"
        />
      </div>

      <div class="counter">{{ counter }}</div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'
import { PROJECTS } from '../config.js'
import PostCard from './PostCard.vue'

const STEP = 600
const ANIM_DUR = 520

const track      = ref(null)
const stage      = ref(null)
const cardRefs   = ref([])
const stageHeight= ref(300)
const counter    = ref('')
const trackHeight= ref(PROJECTS.length * STEP + STEP)

let current   = -1
let animating = false
let lastScrollY = 0

function isMobile() { return window.innerWidth <= 640 }

function getCardEl(i) {
  return cardRefs.value[i]?.$el ?? cardRefs.value[i]
}

function calibrate() {
  const els = PROJECTS.map((_, i) => getCardEl(i)).filter(Boolean)
  els.forEach(el => {
    el.style.opacity    = '1'
    el.style.transform  = 'none'
    el.style.position   = 'relative'
  })
  nextTick(() => {
    const h = Math.max(...els.map(el => el.offsetHeight), 200)
    stageHeight.value = h
    els.forEach((el, i) => {
      el.style.position = 'absolute'
      el.style.top      = '0'
      el.style.left     = '0'
      el.style.opacity  = '0'
      el.style.transform = `translateY(80px) rotate(${PROJECTS[i].rot * 1.8}deg) scale(.93)`
      el.style.transition = 'none'
    })
    if (current >= 0) showCard(current)
  })
}

function showCard(i) {
  const el  = getCardEl(i)
  const rot = PROJECTS[i].rot
  if (!el) return
  el.style.opacity   = '1'
  el.style.transform = `rotate(${rot}deg)`
}

function setCard(idx, direction) {
  if (idx === current || animating) return
  if (idx < 0 || idx >= PROJECTS.length) return

  animating = true
  const prev = current
  current    = idx

  const entering = getCardEl(idx)
  const leaving  = prev >= 0 ? getCardEl(prev) : null
  const mob      = isMobile()

  if (leaving) {
    const exitY   = direction > 0 ? -70 : 90
    const leavRot =  PROJECTS[prev].rot + (direction > 0 ? -2 : 2)
    leaving.style.transition = `opacity ${ANIM_DUR}ms cubic-bezier(.4,0,1,1), transform ${ANIM_DUR}ms cubic-bezier(.4,0,1,1)`
    leaving.style.opacity    = '0'
    leaving.style.transform  = `translateY(${exitY}px) rotate(${leavRot}deg) scale(.94)`
    leaving.classList.remove('active')
  }

  const enterY   = direction > 0 ? 90 : -70
  const startRot = PROJECTS[idx].rot * 1.6
  const restRot  = PROJECTS[idx].rot

  entering.style.transition = 'none'
  entering.style.opacity    = '0'
  entering.style.transform  = `translateY(${enterY}px) rotate(${startRot}deg) scale(.92)`

  requestAnimationFrame(() => requestAnimationFrame(() => {
    entering.style.transition = `opacity ${ANIM_DUR}ms cubic-bezier(.2,.9,.3,1.1), transform ${ANIM_DUR}ms cubic-bezier(.2,.9,.28,1.12)`
    entering.style.opacity    = '1'
    entering.style.transform  = `rotate(${restRot}deg)`
    entering.classList.add('active')
    setTimeout(() => { animating = false }, ANIM_DUR)
  }))

  counter.value = `${idx + 1} / ${PROJECTS.length}`
}

function getTargetIdx() {
  if (!track.value) return -1
  const scrolled = -track.value.getBoundingClientRect().top
  if (scrolled < 0) return -1
  return Math.min(Math.floor(scrolled / STEP), PROJECTS.length - 1)
}

function onScroll() {
  const dir = window.scrollY > lastScrollY ? 1 : -1
  lastScrollY = window.scrollY
  const idx = getTargetIdx()
  if (idx >= 0) setCard(idx, dir)
}

let obs
function setupObserver() {
  obs = new IntersectionObserver(entries => {
    if (entries[0].isIntersecting && current === -1) {
      setCard(0, 1)
    }
  }, { threshold: 0.1 })
  obs.observe(track.value)
}

onMounted(() => {
  lastScrollY = window.scrollY
  window.addEventListener('scroll', onScroll,  { passive: true })
  window.addEventListener('resize', calibrate, { passive: true })
  nextTick(() => {
    calibrate()
    setupObserver()
  })
})
onUnmounted(() => {
  window.removeEventListener('scroll', onScroll)
  window.removeEventListener('resize', calibrate)
  obs?.disconnect()
})
</script>

<style scoped>
.track 
{
  position: relative;
  z-index: 10;
}

.sticky 
{
  position: sticky;
  top: 0;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.section-lbl 
{
  font-family: celeste;
  font-size: .62rem;
  letter-spacing: .35em;
  color: rgba(180,210,255,.5);
  text-transform: uppercase;
  text-shadow: 0 0 .75rem rgba(100,160,255,.4);
  margin-bottom: 1.75rem;
  animation: pulse 3s ease-in-out infinite;
}

.stage 
{
  position: relative;
  width: min(42.5rem, 90vw);
}
.card-slot 
{
  position: absolute;
  top: 0; left: 0;
  width: 100%;
  will-change: transform, opacity;
  pointer-events: none;
}

.card-slot.active 
{ 
  pointer-events: auto; 
}

.counter 
{
  margin-top: 3.5rem;
  font-family: celeste;
  font-size: .58rem;
  letter-spacing: .25em;
  color: rgba(180,210,255,.4);
  text-transform: uppercase;
  min-height: 1em;
}

@media (width <= 648px) 
{
  .counter
  {
    margin-top: 50%;
  }  
}
</style>
