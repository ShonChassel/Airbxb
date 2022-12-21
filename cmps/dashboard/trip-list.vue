<template>
    <div class="trip-list">
        <trip-preview :class="{isSelected: currTrip._id === trip._id}" @click="openToDisplay(trip)" v-for="trip in trips" :trip="trip" :key="trip._id"/>
    </div>
</template>
  
<script>
import tripPreview from "./trip-preview.vue";

export default {
    props: {
        trips: {
            type: Array,
        },
    },
    data(){
        return{
            currTrip: this.trips[0]
        }
    },
    components: {
        tripPreview,
    },
    methods:{
        openToDisplay(trip){
            this.currTrip = trip
            this.$emit('openToDisplay',trip)
        }
    }
};
</script>