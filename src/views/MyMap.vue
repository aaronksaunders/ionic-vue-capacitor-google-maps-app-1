<script setup lang="ts">
import { onMounted, nextTick, ref } from "vue";
import { GoogleMap } from '@capacitor/google-maps';

const mapRef = ref<HTMLElement>();
let newMap: GoogleMap;
onMounted(async () => {
  nextTick(async () => {
    await createMap();
  });
});

async function createMap() {
  if (!mapRef.value) return;
  newMap = await GoogleMap.create({
    id: "my-cool-map",
    element: mapRef.value,
    apiKey: import.meta.env.VITE_APP_YOUR_API_KEY_HERE as string,
    config: {
      center: {
        lat: 33.6,
        lng: -117.9,
      },
      zoom: 8,
    },
  });
}
</script>
<template>
  <div style="width:100%; height:100%">
    <capacitor-google-map
      ref="mapRef"
      style="display: inline-block; width: 100vw; height: 94vh;">
      </capacitor-google-map>

  </div>
</template>
