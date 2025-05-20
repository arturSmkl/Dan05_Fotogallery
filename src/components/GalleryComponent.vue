<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import { searchPhotos } from "@/servicies/pexels.js";
import FullImage from "@/components/FullImage.vue";
import ImageDesc from "@/components/ImageDesc.vue";

const imagesObj = ref([]) // Array of objects received in the response with image urls and photographer name
const isLoading = ref(false);
let cols = 4;  // Number of columns in the grid
let rows = 0;  // Number of rows in the grid
let page = 1; // Current page of the API
const gridTemplate = ref("");  // Grid template style for the container

const overlay = ref(false); // Overlay for the full image
const currentImageIdx = ref(0);  // Index of the image clicked by the user to show the overlay

const descVisible = ref(false);  // Visibility of the description component
const descText = ref("");  // Name of the photographer
const descStyle = ref("");  // Style of the description component - positioning it near the mouse cursor

// Called when the user scrolls to the bottom of the page
function updateGridTemplate(rowsToAdd){
  rows += rowsToAdd;

  const gtc = `grid-template-columns: repeat(${cols}, 1fr);`
  const gtr = `grid-template-rows: repeat(${rows}, 1fr);`

  gridTemplate.value = `${gtc} ${gtr} aspect-ratio: ${cols}/${rows};`;
}


// Function to load images from the API
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


// Function to show the overlay with the full image
function showOverlay(imageIdx) {
  overlay.value = true;
  currentImageIdx.value = imageIdx;
}


// Called when the user scrolls
function handleScroll() {
  // Check if the user has scrolled to the bottom of the page
  const bottomReached = window.innerHeight + window.scrollY >= document.body.offsetHeight - 50;
  if (bottomReached && !isLoading.value) {
    // Load more rows of images
    updateGridTemplate(2);
    loadRows(2);
  }
}


// Function to show the description component
function showDesc(event, name) {
  descText.value = name;
  descStyle.value = `top: ${event.clientY + 15}px; left: ${event.clientX + 15}px;`;
  descVisible.value = true;
}


// Function to move the description component with the mouse
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