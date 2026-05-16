<template>
  <canvas ref="canvas" class="snow"/>
  <canvas ref="canvasBlur" class="snow-blurred"/>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const canvas = ref(null);
const canvasBlur = ref(null);
let raf, W, H;
let targetX = 0;
let currentX = 0;
const LERP = .06;
const SHIFT_NEAR = 28;
const SHIFT_FAR = 12;

let gyrobase = null;

const N = 80;
const flakes = Array.from({ length: N }, () => 
({
  x: Math.random(),
  y: Math.random(),
  r: .8 + Math.random() *  3,
  sp: .25 + Math.random() * .6,
  dx: (Math.random() - .5) * .28,
  op: .22 + Math.random() * .25,
  blurred: Math.random() < .35
}));

function onMouseMove(e)
{
  targetX = (e.clientX / window.innerWidth) * 2 - 1;
}

function resize()
{
  W = canvas.value.width = canvasBlur.value.width = window.innerWidth
  H = canvas.value.height = canvasBlur.value.height = window.innerHeight
}

function draw()
{
  currentX += (targetX - currentX) * LERP;
  canvas.value.style.transform = `translateX(${-currentX * SHIFT_NEAR}px)`
  canvasBlur.value.style.transform = `translateX(${-currentX * SHIFT_FAR}px)`

  const ctx = canvas.value.getContext('2d');
  const ctx2 = canvasBlur.value.getContext('2d');

  ctx.clearRect(0,0,W,H);
  ctx2.clearRect(0,0,W,H);

  for(const f of flakes)
  {
    const c = f.blurred ? ctx2 : ctx;
    c.beginPath();
    c.arc(f.x * W, f.y * H, f.r, 0, Math.PI * 2);
    c.fillStyle = `rgba(210,232,255,255)`;
    c.fill();
    
    f.y += f.sp / H * 1.8;
    f.x += f.dx / W;

    if(f.y > 1.01)
    {
      f.y = -0.01;
      f.x = Math.random();
    }

    if(f.x < 0) f.x = 1;
    if(f.y > 1) f.x = 0;
  }

  raf = requestAnimationFrame(draw);
}

onMounted(() => 
{
  resize();
  window.addEventListener('resize', resize, {passive: true});
  window.addEventListener('mousemove', onMouseMove, { passive: true });
  draw();
});

onUnmounted(() => 
{
  cancelAnimationFrame(raf);
  window.removeEventListener('resize', resize);
  window.removeEventListener('mousemove', onMouseMove);
});
</script>

<style scoped>
.snow, .snow-blurred
{
  position: fixed;
  z-index: 1;

  top: 0;
  left: -5vw;
  width: 110vw;
  height: 110vh;

  will-change: transform;
  pointer-events: none;
}

.snow-blurred
{
  filter: blur(.25rem);
}
</style>