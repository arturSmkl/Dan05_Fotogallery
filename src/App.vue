<script setup>

import HeaderComponent from "@/components/HeaderComponent.vue";
import GalleryComponent from "@/components/GalleryComponent.vue";
</script>

<template>
<HeaderComponent />
<main>
 <GalleryComponent />
</main>
<footer>
  @ 2025 Artur Smýkal
</footer>
</template>

<style scoped>
main {
  width: 1400px;
  margin: 4rem auto;
}

footer {
  width: 100%;
  height: 2.5rem;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: var(--vt-c-black-mute);
  position: sticky;
  bottom: 0;
}
</style>
