<template>
  <v-container class="fill-height">
    <v-responsive class="align-centerfill-height mx-auto" max-width="900">
      <v-tabs v-model="tab" grow color="primary">
        <v-tab v-for="item in tabNames" :key="item" :text="item" :value="item"></v-tab>
      </v-tabs>
      <v-tabs-window v-model="tab">
        <v-tabs-window-item value="USA">
          <v-container class="fill-height">
            <v-responsive class="align-centerfill-height mx-auto" max-width="900">
              <div class="text-h5 text-center">Last Update: December 14, 2025</div>
              <div class="text-h6 text-center">Number of Photos: {{ numberOfUSAImages }}</div>
              <v-carousel hide-delimiters>
                <v-carousel-item v-for="number in usaNumbersArray" :key="number" :src="usaImages[number]"
                  alt="35mm Photo"></v-carousel-item>
              </v-carousel>
            </v-responsive>
          </v-container>
        </v-tabs-window-item>
        <v-tabs-window-item value="Paris">
          <v-container class="fill-height">
            <v-responsive class="align-centerfill-height mx-auto" max-width="900">
              <div class="text-h5 text-center">Last Update: December 14, 2025</div>
              <div class="text-h6 text-center">Number of Photos: {{ numberOfParisImages }}</div>
              <v-carousel hide-delimiters>
                <v-carousel-item v-for="number in parisNumbersArray" :key="number" :src="parisImages[number]"
                  alt="35mm Photo"></v-carousel-item>
              </v-carousel>
            </v-responsive>
          </v-container>
        </v-tabs-window-item>
      </v-tabs-window>
    </v-responsive>
  </v-container>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const numberOfUSAImages = 188;
const numberOfParisImages = 53;

const uInt8ArrayUSA = new Uint8Array(numberOfUSAImages);
const uInt8ArrayParis = new Uint8Array(numberOfParisImages);

window.crypto.getRandomValues(uInt8ArrayUSA);
window.crypto.getRandomValues(uInt8ArrayParis);

const usaNumbersArray = Array.from({ length: numberOfUSAImages }, (_, i) => i);
const parisNumbersArray = Array.from({ length: numberOfParisImages }, (_, i) => i);

// Shuffle the usaNumbersArray
for (let i = 0; i < numberOfUSAImages; i += 1) {
  let swapIndex = uInt8ArrayUSA[i] % numberOfUSAImages;
  let swapValue = usaNumbersArray[swapIndex];
  usaNumbersArray[swapIndex] = usaNumbersArray[i];
  usaNumbersArray[i] = swapValue;
}
// Shuffle the parisNumbersArray
for (let i = 0; i < numberOfParisImages; i += 1) {
  let swapIndex = uInt8ArrayParis[i] % numberOfParisImages;
  let swapValue = parisNumbersArray[swapIndex];
  parisNumbersArray[swapIndex] = parisNumbersArray[i];
  parisNumbersArray[i] = swapValue;
}

const usaImages = ref({});
const parisImages = ref({});

onMounted(async () => {
  const usaImagePromises = usaNumbersArray.map(async (number) => {
    try {
      const image = await import(`@/assets/images/usa/${number}.jpeg`);
      usaImages.value[number] = image.default; // Store image URL in the object
    } catch (error) {
      console.error(`Error loading image ${number}:`, error);
    }
  });
  const parisImagePromises = parisNumbersArray.map(async (number) => {
    try {
      const image = await import(`@/assets/images/paris/${number}.jpeg`);
      parisImages.value[number] = image.default; // Store image URL in the object
    } catch (error) {
      console.error(`Error loading image ${number}:`, error);
    }
  });

  await Promise.all(usaImagePromises); // Wait for all images to load
  await Promise.all(parisImagePromises); // Wait for all images to load
});

const tab = ref('Cities');

const tabNames = [
  'Paris',
  'USA',
];
</script>
