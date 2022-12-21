<template>
<section class="search-modal-mobile">
<section class="search-mobile-inputs">
    <button class="back-btn">&lt; Back to app</button>
    <input @click="setZone('Where')" type="text" placeholder="Where">
    <input @click="setZone('Dates')" type="text" placeholder="Dates">
    <input @click="setZone('Guests')" type="text" placeholder="Guests">
</section>
<section class="modals">
    <destination-modal :class="{ open: currZone === 'Where' }" />
    <guests-modal :class="{ open: currZone === 'Who' }"/>
</section>
    
</section>
</template>

<script>
import destinationModal from "./destination-modal.vue";
import datePicker from "./date-picker.vue";
import guestsModal from "./guests-modal.vue";
    export default {
        data(){
            return{
                currZone: ''
            }
        },
        methods:{
            setZone(value){
                this.currZone = value
            }
        },
        components: {
            destinationModal,
            datePicker,
            guestsModal
        }
    }
</script>
