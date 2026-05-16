<template>
  <div class="bg">
    <img class="bg-d" src="/desktop-bgm.png" alt=""/>
    <img class="bg-m" src="/mobile-bgm.png"  alt=""/>
  </div>
</template>

<style scoped>
.bg 
{
  position: fixed;
  inset: 0;
  z-index: 0;
  pointer-events: none;
}

.bg-d, .bg-m 
{
  position: absolute;
  inset: 0;
  width: 100%; height: 100%;
  object-fit: cover;
  object-position: top center;
}

.bg-m 
{ 
  display: none; 
}

.bg::after 
{
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(10,14,42,.1) 0%, rgba(6,10,30,.25) 100%);
}

@media (max-width: 640px) 
{
  .bg-d 
  { 
    display: none; 
  }

  .bg-m 
  { 
    display: block;
  }
}
</style>
