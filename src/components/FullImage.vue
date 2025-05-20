<script setup>
import {onBeforeUnmount, onMounted, ref, watch} from "vue";

const props = defineProps({
  imagesObj: Array,
  currentImageIdx: Number
})

const emit = defineEmits(["close"])

const localImageIdx = ref(props.currentImageIdx)

watch(() => props.currentImageIdx, (newVal) => {
  localImageIdx.value = newVal
});

function nextImage() {
  if (localImageIdx.value < props.imagesObj.length - 1) {
    localImageIdx.value++
  }
}

function prevImage() {
  if (localImageIdx.value > 0) {
    localImageIdx.value--
  }
}

function handleKeydown(event) {
  if (event.key === "ArrowRight") {
    nextImage()
  } else if (event.key === "ArrowLeft") {
    prevImage()
  } else if (event.key === "Escape") {
    emit("close") // Optional: close overlay with ESC key
  }
}

onMounted(() => {
  window.addEventListener("keydown", handleKeydown)
})

onBeforeUnmount(() => {
  window.removeEventListener("keydown", handleKeydown)
})

</script>

<template>
  <div class="overlay">
    <div class="container">
      <button class="close" @click="emit('close')" />
      <button class="arrow right-arrow" @click=nextImage() />
      <button class="arrow left-arrow" @click=prevImage() />
      <img :src=props.imagesObj[localImageIdx].src.original alt="hot babe" />
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
  background-color: var(--vt-c-black-trans);
}

.container {
  height: 95vh;
  aspect-ratio: 1/1;
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
  transition: all 0.1s ease-in-out;
}

button:hover {
  transform: scale(1.25);
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

@media (orientation: portrait) {
  .container {
    height: 95vw;
  }
}

</style>