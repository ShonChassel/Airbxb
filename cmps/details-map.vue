<template>
    <GoogleMap v-if="center" api-key="AIzaSyDJ5CRpSy0V14lOUli9vStS6lCjaSStmNU"
        :center="center" :zoom="10">
        <Marker :options="marker" />
    </GoogleMap>
</template>
  
<script>
import { GoogleMap, Marker } from 'vue3-google-map'

export default {
    props: {
        lat: Number,
        lng: Number,
        title: String,
    },
    components: { GoogleMap, Marker },
    data() {
        return {
            center: { lat: 31.7683, lng: 35.2137 },
            marker: null
        }
    },
    created() {
        this.center.lat = +this.lat
        this.center.lng = +this.lng
        const marker = {
            title: this.title,
            position: this.center
        }
        this.marker = marker
    }

}
</script>
  