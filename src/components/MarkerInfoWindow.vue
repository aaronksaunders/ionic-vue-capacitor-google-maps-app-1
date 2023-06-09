<template>
  <ion-content style="--width: 90%">
    <p>{{JSON.stringify(marker,null,2)}}</p>
  </ion-content>
</template>
<script setup lang="ts">
import {
  IonButton,
  IonCol,
  IonContent,
  IonGrid,
  IonIcon,
  IonLabel,
  IonNote,
  IonRow,
} from "@ionic/vue";

// PROPS
const props = defineProps<{
  marker: { coordinate: any; title: string };
}>();
const emits = defineEmits<{
  (event: "onDismiss"): void;
}>();
</script>
<style></style>
