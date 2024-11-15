<template>
  <v-container class="fill-height">
    <v-responsive class="align-centerfill-height mx-auto" max-width="900">
      <div class="text-h5 text-center">Last Update: November 15, 2024</div>
      <div class="text-h6 text-center">Number of Photos: {{ numberOfImages }}</div>
      <v-carousel hide-delimiters>
        <v-carousel-item v-for="number in numbersArray" :key="number" :src="images[number]"
          alt="35mm Photo"></v-carousel-item>
      </v-carousel>
    </v-responsive>
  </v-container>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const numberOfImages = 148;
const uInt8Array = new Uint8Array(numberOfImages);
window.crypto.getRandomValues(uInt8Array);
const numbersArray = Array.from({ length: numberOfImages }, (_, i) => i);

// Shuffle the numbersArray
for (let i = 0; i < numberOfImages; i += 1) {
  let swapIndex = uInt8Array[i] % numberOfImages;
  let swapValue = numbersArray[swapIndex];
  numbersArray[swapIndex] = numbersArray[i];
  numbersArray[i] = swapValue;
}
const images = ref({});

onMounted(async () => {
  const imagePromises = numbersArray.map(async (number) => {
    try {
      const image = await import(`@/assets/images/${number}.jpeg`);
      images.value[number] = image.default; // Store image URL in the object
    } catch (error) {
      console.error(`Error loading image ${number}:`, error);
    }
  });

  await Promise.all(imagePromises); // Wait for all images to load
});
</script>
