<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab anxing123 {{text}}</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 1</ion-title>
        </ion-toolbar>
      </ion-header>

      <ExploreContainer name="Tab 1 page" />
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
import { ref } from 'vue'

let text = ref<string>('')
setInterval(() => {
  text.value = `${Date.now()}`
}, 3000)
</script>
