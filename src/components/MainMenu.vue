<script>
import { computed } from 'vue'

export default {
    // emits: ['closeMenu'],

    //Nota The props parameter doesn’t have a default value, so we have to include it even if we don’t use it.
    setup(props, context) {

        function sendEvent() {
            context.emit('closeMenu');
        }

        return {
            sendEvent
        }
    }
}
</script>

<template>
    <p>Main menu component:</p>
    <button @click="sendEvent">Close Menu</button>
</template>