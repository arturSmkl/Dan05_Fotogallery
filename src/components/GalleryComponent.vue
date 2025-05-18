<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import { searchPhotos } from "@/servicies/pexels.js";

const images = ref([])
const isLoading = ref(false);
let cols = 4;
let rows = 0;
let page = 1;

const gridTemplate = ref("");

function updateGridTemplate(rowsToAdd){
  rows += rowsToAdd;

  const gtc = `grid-template-columns: repeat(${cols}, 1fr);`
  const gtr = `grid-template-rows: repeat(${rows}, 1fr);`

  gridTemplate.value = `${gtc} ${gtr} aspect-ratio: ${cols}/${rows};`;
}

async function loadRows(rowsToLoad){
  isLoading.value = true;
  for (let i = 0; i < rowsToLoad; i++) {
    let data = await searchPhotos('homosexual women', page, cols)
    for (let d of data){
      images.value.push(`background-image: url("${d.src.large}");`);
    }
    page++;
  }
  isLoading.value = false;
}


function handleScroll() {
  const bottomReached = window.innerHeight + window.scrollY >= document.body.offsetHeight - 50;
  if (bottomReached && !isLoading.value) {
    updateGridTemplate(2);
    loadRows(2);
  }
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
  <div class="container" :style=gridTemplate>
    <div class="item" v-for="(i, idx) in images" :key="idx" :style=i ></div>
  </div>
</template>

<style scoped>
  .container {
    display: grid;
    width: 100%;
    align-items: center;
    justify-items: center;
  }

  .item {
    width: 95%;
    aspect-ratio: 1/1;
    background-size: cover;
    background-position: center;
    border-radius: 1rem;
  }

</style>