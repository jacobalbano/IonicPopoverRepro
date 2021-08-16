<template>
    <ion-popover
        :is-open="isOpenRef"
        css-class="dialog-modoki-popover"
        translucent="true"
        :event="event"
        :enter-animation="enterAnimation"
        :leave-animation="leaveAnimation"
        @didDismiss="resolve({ role: 'cancel' })"
    >
        <div>
            <slot></slot>
            <div class="button-slot">
                <slot name="buttons">
                    <ion-button @click="resolve({ role: 'cancel'})" fill="clear">Cancel</ion-button>
                    <ion-button @click="resolve({ role: 'ok'})" fill="clear">Ok</ion-button>
                </slot>
            </div>
        </div>
    </ion-popover>
</template>
<script>
import { defineComponent, ref } from 'vue';
import { createAnimation, IonPopover, IonButton } from '@ionic/vue'

export default defineComponent({
    name: 'base-dialog',
    components: {
        IonPopover,
        IonButton
    },
    methods: {
        async show() {
            return this.setOpen(true);
        },

        enterAnimation: e => fade(e),
        leaveAnimation: e => fade(e).direction('reverse')
    },

    setup() {
        let resolvePromise = null;
        const isOpenRef = ref(false);
        const event = ref();

        return { isOpenRef, setOpen, event, resolve };
        
        function resolve(x) {
            if (resolvePromise != null) resolvePromise(x);
            resolvePromise = null;
            setOpen(false);
        }

        function setOpen(state, ev) {
            event.value = ev; 
            isOpenRef.value = state;
            if (state) {
                return new Promise(r => resolvePromise = r);
            }
        }
    }
})

function fade(baseEl) {
    const backdropAnimation = createAnimation()
        .addElement(baseEl.querySelector('ion-backdrop'))
        .fromTo('opacity', '0.01', 'var(--backdrop-opacity)');

    const wrapperAnimation = createAnimation()
        .addElement(baseEl.querySelector('.popover-wrapper'))
        .keyframes([
            { offset: 0, opacity: '0' },
            { offset: 1, opacity: '1' }
        ]);

    return createAnimation()
        .addElement(baseEl)
        .easing('ease-out')
        .duration(200)
        .addAnimation([backdropAnimation, wrapperAnimation]);
}
</script>
<style>
.dialog-modoki-popover .popover-content {
    position: unset;
    width: unset;
}

.button-slot {
  display: flex;
  justify-content: flex-end;
}
</style>