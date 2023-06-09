<script setup lang="ts">
import { onMounted, nextTick, ref, onUnmounted } from "vue";
import { GoogleMap } from "@capacitor/google-maps";
import { onIonViewWillLeave, IonPopover, IonContent } from "@ionic/vue";
import { CapacitorGoogleMaps } from "@capacitor/google-maps/dist/typings/implementation";

// PROPS
const props = defineProps<{
  markerData: { coordinate: any; title: string; snippet: string }[];
}>();
const emits = defineEmits<{
  (event: "onMarkerClicked", info: any): void;
  (event: "onMapClicked"): void;
}>();

const mapRef = ref<HTMLElement>();
const markerIds = ref<string[] | undefined>();
let newMap: GoogleMap;

onMounted(async () => {
  console.log("mounted ", mapRef.value);
  await nextTick();
  await createMap();
});

onUnmounted(() => {
  console.log("onunmounted");
  newMap.removeMarkers(markerIds?.value as string[]);
});

const addSomeMarkers = async (newMap: GoogleMap) => {
  markerIds?.value && newMap.removeMarkers(markerIds?.value as string[]);

  // Plot each point on the map
  let markers = props.markerData.map(({ coordinate, title, snippet }) => {
    return {
      coordinate,
      title,
      snippet
    };
  });

  markerIds.value = await newMap.addMarkers(markers);
};

async function createMap() {
  if (!mapRef.value) return;

  newMap = await GoogleMap.create({
    id: "my-cool-map",
    element: mapRef.value,
    apiKey: import.meta.env.VITE_APP_YOUR_API_KEY_HERE as string,
    config: {
      center: {
        lat: 37.783,
        lng: -122.408,
      },
      zoom: 12,
    },
  });

  addSomeMarkers(newMap);

  // Handle marker click
  newMap.setOnMarkerClickListener((event) => {
    debugger
    emits("onMarkerClicked", event);
  });

  newMap.setOnMapClickListener(() => {
    debugger;
    emits("onMapClicked");
  });
}
</script>
<template>
  <div>
    <capacitor-google-map
      ref="mapRef"
      style="display: inline-block; width: 100vw; height: 86vh"
    >
    </capacitor-google-map>

  </div>
</template>
