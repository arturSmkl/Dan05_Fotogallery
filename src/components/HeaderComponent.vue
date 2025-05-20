<script setup>

</script>

<template>
<header>
  <img src="../assets/images/heart.png" alt="heart">
  <h1 class="grad-text">
    HOT BABES
  </h1>
  <h2>
    Explore the beauty of women
  </h2>
</header>
</template>

<style scoped>

header {
  height: 20rem;
  width: 90rem;
  border-bottom-right-radius: 20rem;
  border-bottom-left-radius: 20rem;
  display: flex;
  flex-direction: column;
  justify-content: end;
  align-items: center;
  padding: 1.5rem;
  margin: 0 auto;
  background: var(--vt-c-black-soft);
}

img {
  width: 8rem;
  aspect-ratio: 1 / 1;
}

@media (max-width: 1000px) {
  header {
    width: 100%;
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 0;
    height: 14rem;
  }

  img {
    width: 5rem;
  }
}

</style>