<template>
  <div class="net-bg">
    <canvas ref="cvs" />
  </div>
</template>

<script setup>
import { ref, watch, onMounted, onUnmounted } from 'vue'
import { useNav } from '@slidev/client'

const { currentPage } = useNav()
const cvs = ref(null)
let animId = null
const opacity = ref(0.55)

watch(currentPage, (p) => {
  opacity.value = p === 1 ? 0.55 : 0.15
}, { immediate: true })

onMounted(() => {
  const canvas = cvs.value
  if (!canvas) return
  const ctx = canvas.getContext('2d')
  canvas.width = 960
  canvas.height = 540
  const W = canvas.width, H = canvas.height

  // Barabási–Albert preferential attachment
  const N = 300, INIT = 6, M = 3
  const nodes = [], edges = []

  for (let i = 0; i < INIT; i++) {
    nodes.push({
      x: W / 2 + (Math.random() - .5) * 140,
      y: H / 2 + (Math.random() - .5) * 100,
      vx: (Math.random() - .5) * .18,
      vy: (Math.random() - .5) * .18,
      deg: INIT - 1
    })
  }
  for (let i = 0; i < INIT; i++)
    for (let j = i + 1; j < INIT; j++)
      edges.push([i, j])

  for (let i = INIT; i < N; i++) {
    nodes.push({
      x: Math.random() * W,
      y: Math.random() * H,
      vx: (Math.random() - .5) * .18,
      vy: (Math.random() - .5) * .18,
      deg: 0
    })
    const chosen = new Set()
    for (let e = 0; e < Math.min(M, i); e++) {
      let total = 0
      for (let j = 0; j < i; j++) if (!chosen.has(j)) total += nodes[j].deg + 1
      let r = Math.random() * total
      for (let j = 0; j < i; j++) {
        if (chosen.has(j)) continue
        r -= nodes[j].deg + 1
        if (r <= 0) {
          chosen.add(j)
          edges.push([i, j])
          nodes[i].deg++
          nodes[j].deg++
          break
        }
      }
    }
  }

  const maxDeg = Math.max(...nodes.map(n => n.deg))

  const draw = () => {
    ctx.clearRect(0, 0, W, H)

    for (const [a, b] of edges) {
      const t = (nodes[a].deg + nodes[b].deg) / (2 * maxDeg)
      ctx.strokeStyle = `rgba(24,46,54,${.04 + t * .13})`
      ctx.lineWidth = .4 + t * .6
      ctx.beginPath()
      ctx.moveTo(nodes[a].x, nodes[a].y)
      ctx.lineTo(nodes[b].x, nodes[b].y)
      ctx.stroke()
    }

    for (const n of nodes) {
      const t = n.deg / maxDeg
      ctx.fillStyle = `rgba(24,46,54,${.15 + t * .6})`
      ctx.beginPath()
      ctx.arc(n.x, n.y, 1.5 + Math.sqrt(t) * 8, 0, Math.PI * 2)
      ctx.fill()
      n.x += n.vx; n.y += n.vy
      if (n.x < 0 || n.x > W) n.vx *= -1
      if (n.y < 0 || n.y > H) n.vy *= -1
    }

    animId = requestAnimationFrame(draw)
  }
  draw()
})

onUnmounted(() => { if (animId) cancelAnimationFrame(animId) })
</script>

<style>
.slidev-slide,
.slidev-layout {
  background-color: transparent !important;
}
</style>

<style scoped>
.net-bg {
  position: fixed;
  inset: 0;
  z-index: -1;
  pointer-events: none;
  background: white;
  overflow: hidden;
}
.net-bg canvas {
  width: 100%;
  height: 100%;
  display: block;
  opacity: v-bind(opacity);
  transition: opacity 0.6s ease;
}
</style>
