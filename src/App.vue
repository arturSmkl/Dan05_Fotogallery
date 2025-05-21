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
  @ 2025 Artur Smýkal - Specially made for Daniel Sigma Kopecký
</footer>
</template>

<style scoped>
main {
  width: 1400px;
  margin: 4rem auto;
  min-height: 100vh;
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

@media (max-width: 1500px) {
  main {
    width: 90%;
  }

  footer {
    font-size: 0.8rem;
  }

}

</style>
