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
        <v-tabs-window-item value="Boston">
          <v-container class="fill-height">
            <v-responsive class="align-centerfill-height mx-auto" max-width="900">
              <div class="text-h5 text-center">Last Update: May 5, 2026</div>
              <div class="text-h6 text-center">Number of Photos: {{ numberOfBostonImages }}</div>
              <v-carousel hide-delimiters>
                <v-carousel-item v-for="number in bostonNumbersArray" :key="number" :src="bostonImages[number]"
                  alt="35mm Photo"></v-carousel-item>
              </v-carousel>
            </v-responsive>
          </v-container>
        </v-tabs-window-item>
        <v-tabs-window-item value="maJ-Mil!">
          <v-container class="fill-height">
            <v-responsive class="align-centerfill-height mx-auto" max-width="900">
              <div class="text-h5 text-center">Last Update: June 3, 2026</div>
              <div class="text-h6 text-center">Number of Photos: {{ numberOfMajimaImages }}</div>
              <v-carousel hide-delimiters>
                <v-carousel-item v-for="number in majimaNumbersArray" :key="number" :src="majimaImages[number]"
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
const numberOfBostonImages = 5;
const numberOfMajimaImages = 5;

const uInt8ArrayUSA = new Uint8Array(numberOfUSAImages);
const uInt8ArrayParis = new Uint8Array(numberOfParisImages);
const uInt8ArrayBoston = new Uint8Array(numberOfBostonImages);
const uInt8ArrayMajima = new Uint8Array(numberOfMajimaImages);

window.crypto.getRandomValues(uInt8ArrayUSA);
window.crypto.getRandomValues(uInt8ArrayParis);
window.crypto.getRandomValues(uInt8ArrayBoston);
window.crypto.getRandomValues(uInt8ArrayMajima);

const usaNumbersArray = Array.from({ length: numberOfUSAImages }, (_, i) => i);
const parisNumbersArray = Array.from({ length: numberOfParisImages }, (_, i) => i);
const bostonNumbersArray = Array.from({ length: numberOfBostonImages }, (_, i) => i);
const majimaNumbersArray = Array.from({ length: numberOfMajimaImages }, (_, i) => i);

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
// Shuffle the bostonNumbersArray
for (let i = 0; i < numberOfBostonImages; i += 1) {
  let swapIndex = uInt8ArrayBoston[i] % numberOfBostonImages;
  let swapValue = bostonNumbersArray[swapIndex];
  bostonNumbersArray[swapIndex] = bostonNumbersArray[i];
  bostonNumbersArray[i] = swapValue;
}

// Shuffle the majimaNumbersArray
for (let i = 0; i < numberOfMajimaImages; i += 1) {
  let swapIndex = uInt8ArrayBoston[i] % numberOfMajimaImages;
  let swapValue = majimaNumbersArray[swapIndex];
  majimaNumbersArray[swapIndex] = majimaNumbersArray[i];
  majimaNumbersArray[i] = swapValue;
}

const usaImages = ref({});
const parisImages = ref({});
const bostonImages = ref({});
const majimaImages = ref({});

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
  const bostonImagePromises = bostonNumbersArray.map(async (number) => {
    try {
      const image = await import(`@/assets/images/boston/${number}.jpeg`);
      bostonImages.value[number] = image.default; // Store image URL in the object
    } catch (error) {
      console.error(`Error loading image ${number}:`, error);
    }
  });
  const majimaImagePromises = majimaNumbersArray.map(async (number) => {
    try {
      const image = await import(`@/assets/images/majima/${number}.jpeg`);
      majimaImages.value[number] = image.default; // Store image URL in the object
    } catch (error) {
      console.error(`Error loading image ${number}:`, error);
    }
  });

  await Promise.all(usaImagePromises); // Wait for all images to load
  await Promise.all(parisImagePromises); // Wait for all images to load
  await Promise.all(bostonImagePromises); // Wait for all images to load
  await Promise.all(majimaImagePromises); // Wait for all images to load
});

const tab = ref('Cities');

const tabNames = [
  'maJ-Mil!',
  'Boston',
  'Paris',
  'USA',
];
</script>
