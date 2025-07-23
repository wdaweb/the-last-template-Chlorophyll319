<template>
  <h2 class="large grid centered square-grid text-xl">
    <span>專</span>
    <span>注</span>
    <span>從</span>
    <span>來</span>
    <span>不</span>
    <span>是</span>
    <span>能</span>
    <span>力</span>
    <span>，</span>
    <span>而</span>
    <span>是</span>
    <span>一</span>
    <span>個</span>
    <span>又</span>
    <span>一</span>
    <span>個</span>
    <span>的</span>
    <span>選</span>
    <span>擇</span>
    <span>。</span>
  </h2>
</template>
<script setup>
import { animate } from 'animejs'

animate('span', {
  // Property keyframes
  y: [
    { to: '-2.75rem', ease: 'outExpo', duration: 600 },
    { to: 0, ease: 'outBounce', duration: 800, delay: 100 },
  ],
  // Property specific parameters
  rotate: {
    from: '-1turn',
    delay: 0,
  },
  delay: (_, i) => i * 50, // Function based value
  ease: 'inOutCirc',
  loopDelay: 1000,
  loop: true,
})
</script>
