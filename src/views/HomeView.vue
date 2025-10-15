<script setup>
import { onMounted } from 'vue'
import init3DScene from '../Utility/script.js'
import ShowcaseView from './ShowcaseView.vue'

onMounted(() => {
  requestAnimationFrame(() => {
    init3DScene()
  })
})
</script>

<template>
  <main class="home-view">
    <!-- <section class="intro">
      <h1>Grind doesn’t shake. It performs</h1>
    </section> -->

    <section class="product-overview">
      <div class="header-1"><h1>Transform Your Brand with
        <img src="/images/8mediahub_logo.png" width="80" alt="Logo" />
      </h1></div>
      <div class="header-2"><h1>Ignite your Future</h1></div>

      <div class="circular-mask"></div>

      <div class="tooltips">
        <div class="tooltip">
          <div class="icon"><ion-icon name="flash"></ion-icon></div>
          <div class="divider"></div>
          <div class="title"><h2>Built to last</h2></div>
          <div class="description">
            <p>
              Designed to match your pace, GRIND runs all week on a single charge.
              No interruptions, no slowing down.
            </p>
          </div>
        </div>

        <div class="tooltip">
          <div class="icon"><ion-icon name="bluetooth"></ion-icon></div>
          <div class="divider"></div>
          <div class="title"><h2>Stay synced</h2></div>
          <div class="description">
            <p>
              With app integration, GRIND helps you stay consistent.
              Monitor intake, set goals, and make every sip count.
            </p>
          </div>
        </div>
      </div>

      <div class="modal-container"></div>
    </section>

    <!-- <section class="outro">
      <h1>Don’t Just Train — Grind</h1>
    </section> -->

    <ShowcaseView />
  </main>
</template>
