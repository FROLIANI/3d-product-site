<script setup>
import { onMounted, nextTick } from 'vue'
import init3DScene  from '../Utility/script.js'


onMounted(async () => {
  await nextTick()
  init3DScene ()
})
</script>
<template>
<section class="intro">
<h1>Grind does'nt shake. It performs</h1>
</section>

<section class="product-overview">
    <div class="header-1">
        <h1>Every Rep Starts with</h1>
    </div>

    <div class="header-2">
        <h1>Grind Shaker</h1>
    </div>

    <div class="circular-mask"></div>

    <div class="tooltips">
        <div class="tooltip"> 
            <div class="icon">
                <ion-icon name="flash"></ion-icon>
            </div>
            <div class="divider"></div>
            <div class="title">
                <h2>Built to last</h2>
            </div>
            <div class="description">
                <p>Designed to match your pace, GRIND runs all week on sigle charge.
                    No in terruptions, no slowing down
                </p>
            </div>
        </div>

         <div class="tooltip"> 
            <div class="icon">
                <ion-icon name="bluetooth"></ion-icon>
            </div>
            <div class="divider"></div>
            <div class="title">
                <h2>Stay synced</h2>
            </div>
            <div class="description">
                <p>With app intergartion, GRIND helps you stay consistent.
                     Monitor inatke, set goals and make every sip count
                </p>
            </div>
        </div>
    </div>

    <div class="modal-conatiner"></div>
</section>

<section class="outro">
<h1>Don't Just Train - Grind</h1>
</section>

</template>