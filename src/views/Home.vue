<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Blank</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      <div id="container">
        <ion-list>
          <ion-item>{{ { plainTimes, dialogTimes } }}</ion-item>
          <ion-item>
            <clicker-button v-model:times="plainTimes" /></ion-item>
          <ion-item>
            <ion-button @click="$refs.dialog.show()">Show Dialog</ion-button>
          </ion-item>
        </ion-list>
        <base-dialog ref="dialog">
          <clicker-button v-model:times="dialogTimes" />
        </base-dialog>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
import { defineComponent } from 'vue';
import BaseDialog from './BaseDialog.vue'
import ClickerButton from './ClickerButton.vue'

export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    BaseDialog,
    ClickerButton
  },

  data() {
    return {
      plainTimes: 0,
      dialogTimes: 0
    }
  }
});
</script>