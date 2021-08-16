<template>
    <ion-button @click="clickedTimes++">Clicked {{times}} times</ion-button>
</template>
<script>
import { IonButton } from '@ionic/vue';
import { defineComponent } from 'vue'

export default defineComponent({
    props: [ 'times' ],
    emits: ['update:times'],
    components: [ IonButton],
    computed: {
        clickedTimes: {
            get() {
                return this.times;
            },

            set(value) {
                this.$emit('update:times', value);
            }
        }
    }
})
</script>
