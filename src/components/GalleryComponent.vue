<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import { searchPhotos } from "@/servicies/pexels.js";
import FullImage from "@/components/FullImage.vue";
import ImageDesc from "@/components/ImageDesc.vue";

const imagesObj = ref([])
const isLoading = ref(false);
let cols = 4;
let rows = 0;
let page = 1;
const gridTemplate = ref("");

const overlay = ref(false);
const currentImageIdx = ref(0);

const descVisible = ref(false);
const descText = ref("");
const descStyle = ref("");

function updateGridTemplate(rowsToAdd){
  rows += rowsToAdd;

  const gtc = `grid-template-columns: repeat(${cols}, 1fr);`
  const gtr = `grid-template-rows: repeat(${rows}, 1fr);`

  gridTemplate.value = `${gtc} ${gtr} aspect-ratio: ${cols}/${rows};`;
}

async function loadRows(rowsToLoad){
  isLoading.value = true;
  for (let i = 0; i < rowsToLoad; i++) {
    let data = await searchPhotos('bikini', page, cols)
    for (let d of data){
      imagesObj.value.push({
        src: {
          original: d.src.original,
          bgStyle: `background-image: url("${d.src.large}");`,
        },
        photographer: d.photographer,
      });
    }
    page++;
  }
  isLoading.value = false;
}

function showOverlay(imageIdx) {
  overlay.value = true;
  currentImageIdx.value = imageIdx;

}

function handleScroll() {
  const bottomReached = window.innerHeight + window.scrollY >= document.body.offsetHeight - 50;
  if (bottomReached && !isLoading.value) {
    updateGridTemplate(2);
    loadRows(2);
  }
}

function showDesc(event, name) {
  descText.value = name;
  descStyle.value = `top: ${event.clientY + 15}px; left: ${event.clientX + 15}px;`;
  descVisible.value = true;
}

function moveDesc(event) {
  if (descVisible.value) {
    descStyle.value = `top: ${event.clientY + 15}px; left: ${event.clientX + 15}px;`;
  }
}

function hideDesc() {
  descVisible.value = false;
}

onMounted(async () => {
  updateGridTemplate(4);
  await loadRows(4);
  window.addEventListener("scroll", handleScroll);
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
});

</script>

<template>
  <FullImage v-if=overlay :images-obj=imagesObj :current-image-idx=currentImageIdx @close="overlay = false" />
  <ImageDesc v-if=descVisible :name=descText :desc-style=descStyle />
  <div class="container" :style=gridTemplate>
    <button class="item"
            v-for="(i, idx) in imagesObj"
            :key=idx
            :style=i.src.bgStyle
            @click="showOverlay(idx)"
            @mouseenter="(e) => showDesc(e, i.photographer)"
            @mousemove="moveDesc"
            @mouseleave="hideDesc" />
  </div>
</template>

<style scoped>
  .container {
    display: grid;
    width: 100%;
    align-items: center;
    justify-items: center;
  }

  button {
    width: 95%;
    aspect-ratio: 1/1;
    background-size: cover;
    background-position: center;
    border-radius: 1rem;
    border: none;
    cursor: pointer;
    transition: all 0.1s ease-in-out;
  }

  button:hover {
    transform: scale(1.05);
  }

</style>