<script setup>
import { onMounted, onBeforeUnmount } from 'vue'
import * as THREE from 'three'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import Lenis from 'lenis'

let lenis, scene, camera, renderer, model

onMounted(() => {
  gsap.registerPlugin(ScrollTrigger)

  // ---- Smooth scrolling ----
  lenis = new Lenis({
    duration: 1.3,
    smoothWheel: true,
    smoothTouch: false
  })

  const raf = (time) => {
    lenis.raf(time)
    ScrollTrigger.update()
    requestAnimationFrame(raf)
  }
  requestAnimationFrame(raf)

  // ---- 3D Background ----
  const container = document.querySelector('.bg3d')
  renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true })
  renderer.setSize(window.innerWidth, window.innerHeight)
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
  container.appendChild(renderer.domElement)

  scene = new THREE.Scene()
  camera = new THREE.PerspectiveCamera(55, window.innerWidth / window.innerHeight, 0.1, 100)
  camera.position.set(0, 0, 3)

  const light = new THREE.DirectionalLight(0xffffff, 1.2)
  light.position.set(2, 2, 2)
  scene.add(light)
  scene.add(new THREE.AmbientLight(0xffffff, 0.6))

  new GLTFLoader().load('/images/shaker.glb', (gltf) => {
    model = gltf.scene
    model.position.set(0, -0.5, 0)
    model.rotation.set(THREE.MathUtils.degToRad(-15), THREE.MathUtils.degToRad(30), 0)
    model.scale.set(1.2, 1.2, 1.2)
    scene.add(model)
  })

  const animate = () => {
    requestAnimationFrame(animate)
    if (model) model.rotation.y += 0.002
    renderer.render(scene, camera)
  }
  animate()

  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight
    camera.updateProjectionMatrix()
    renderer.setSize(window.innerWidth, window.innerHeight)
  })

  // ---- Text Scroll Animations ----
  const panels = gsap.utils.toArray('.panel')
  panels.forEach((panel) => {
    gsap.fromTo(
      panel,
      { opacity: 0, y: 100 },
      {
        opacity: 1,
        y: 0,
        ease: 'power3.out',
        scrollTrigger: {
          trigger: panel,
          start: 'top 70%',
          end: 'bottom 40%',
          scrub: true
        }
      }
    )
  })

  // ---- Sticky Video ----
  ScrollTrigger.create({
    trigger: '.media__frame',
    start: 'top top',
    end: 'bottom bottom',
    pin: true,
    pinSpacing: false
  })

  // ---- Rotate Model on Scroll ----
  ScrollTrigger.create({
    trigger: '.copy',
    start: 'top bottom',
    end: 'bottom top',
    scrub: true,
    onUpdate: (self) => {
      if (model) model.rotation.y = Math.PI * 2 * self.progress
    }
  })

  ScrollTrigger.refresh()
})

onBeforeUnmount(() => {
  ScrollTrigger.getAll().forEach((st) => st.kill())
  if (lenis) lenis.destroy()
  if (renderer) {
    renderer.dispose()
    const canvas = renderer.domElement
    if (canvas && canvas.parentNode) canvas.parentNode.removeChild(canvas)
  }
})
</script>

<template>
  <section class="showcase">
    <div class="bg3d"></div>

    <div class="grid">
      <!-- LEFT (Sticky Video) -->
      <div class="media">
        <div class="media__frame">
          <video
            class="media__video"
            src="/images/video.mov"
            autoplay
            muted
            loop
            playsinline
          ></video>
        </div>
      </div>

      <!-- RIGHT (Scrolling Panels) -->
      <div class="copy">
        <article class="panel">
          <h2>Effortless Setup</h2>
          <p>Plug and play anywhere — adaptive brightness and layout in seconds.</p>
        </article>

        <article class="panel">
          <h2>Smart Scheduling</h2>
          <p>Deploy campaigns instantly across all your screens with real-time sync.</p>
        </article>

        <article class="panel">
          <h2>Dynamic Context</h2>
          <p>Each display reacts to lighting, crowd movement, and time of day.</p>
        </article>

        <article class="panel">
          <h2>Cinematic Motion</h2>
          <p>Fluid transitions and responsive depth for immersive DOOH storytelling.</p>
        </article>
      </div>
    </div>
  </section>
</template>

<style scoped>
.showcase {
  position: relative;
  background: #0d0d0d;
  color: #fff;
  overflow-x: hidden;
  overflow-y: visible;
  min-height: 400vh;
}

/* 3D Background */
.bg3d {
  position: fixed;
  inset: 0;
  z-index: 0;
  pointer-events: none;
}

/* Grid Layout */
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: start;
  gap: 5vw;
  width: 90%;
  margin: 0 auto;
  padding: 10vh 0;
  z-index: 2;
}

/* Sticky Video */
.media__frame {
  position: sticky;
  top: 10vh;
  height: 80vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
.media__video {
  width: 100%;
  max-height: 80vh;
  border-radius: 20px;
  object-fit: cover;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
}

/* Right Panels */
.copy {
  display: flex;
  flex-direction: column;
  gap: 100vh;
  padding-block: 20vh;
  z-index: 3;
}
.panel {
  opacity: 0;
  transform: translateY(100px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.panel h2 {
  font-size: clamp(2rem, 4vw, 4rem);
  margin-bottom: 1rem;
}
.panel p {
  font-size: clamp(1rem, 1.5vw, 1.25rem);
  color: #d0d0d0;
  line-height: 1.7;
  max-width: 50ch;
}

/* Mobile Responsive */
@media (max-width: 900px) {
  .grid {
    grid-template-columns: 1fr;
  }
  .copy {
    gap: 60vh;
  }
  .media__frame {
    position: relative;
    height: 60vh;
  }
}
</style>
