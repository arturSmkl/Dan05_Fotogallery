<script setup>
const props = defineProps({
  imagesUrl: Array,
  currentImageIdx: Number
})

</script>

<template>
  <div class="overlay">
    <div class="container">
      <button class="close" />
      <button class="arrow right-arrow" />
      <button class="arrow left-arrow" />
      <img :src=props.imagesUrl[props.currentImageIdx] alt="hot woman" />
    </div>
  </div>
</template>

<style scoped>

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container {
  height: 90vh;
  aspect-ratio: 1/1;
  background-color: var(--vt-c-black-mute);
  border-radius: 1rem;
  display: grid;
  grid-template-columns: 5% 90% 5%;
  grid-template-rows: 5% 90% 5%;
  place-items: center;
}

button {
  border: none;
  background-image: url("/src/assets/images/close.png");
  background-size: cover;
  background-color: transparent;
  cursor: pointer;
  width: 80%;
  aspect-ratio: 1/1;
}

.arrow {
  background-image: url("/src/assets/images/arrow.png");
}

.right-arrow {
  grid-area: 2 / 3 / 3 / 4;
}

.left-arrow {
  grid-area: 2 / 1 / 3 / 2;
  transform: rotate(180deg);
}

.close {
  grid-area: 1 / 3 / 2 / 4;
}

img {
  grid-area: 2 / 2 / 3 / 3;
  max-width: 100%;
  max-height: 100%;
  width: auto;
  height: auto;
  object-fit: contain;
  border-radius: 1rem;
}

</style>