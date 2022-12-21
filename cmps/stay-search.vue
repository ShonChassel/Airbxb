<template>
    <section class="main-search-container">
        <div v-if="whichSearch" class="main-search">
            <button @click="openZone($event, 'Where')" class="search-btn">{{ setDestination }}</button>
            <span></span>
            <button @click="openZone($event, 'In')" class="search-btn">{{ setDate }}</button>
            <span></span>
            <button @click="openZone($event, 'Who')" class="search-btn last">{{setGuests}}</button>
            <section @click="openZone($event, 'Where')" class="search-in-mobile">
                <search-icon class="img" />
                <div>
                    <label>Where to?</label>
                    <p>Anywhere &bull; Any week &bull; Add guests</p>
                </div>
            </section>
        </div>
        <div v-else class="start-search">Start your search</div>
        <div class="little-search-icon">
            <img src="../assets/svg/search.svg" class="svg-for-desktop" />
            <img src="../assets/svg/filter-btn.svg" class="svg-for-mobile" />
        </div>
    </section>
</template>
  
<script>
import searchIcon from '../assets/svg/search.vue'

export default {
    props: {
        dest: String,
        date: String,
        guests: {
            type: Number,
            default: 0,
        }
    },
    methods: {
        openZone(ev, key) {
            this.$emit('toggleSearch',  ev, key )
        }
    },
    computed: {
        setDate() {
            if (this.date) return this.date
            return 'Any week'
        },
        setDestination() {
            if (this.dest) return this.dest
            return 'Anywhere'
        },
        setGuests() {
            if (this.guests) return `${this.guests} ${this.guests > 1 ? 'guests':'guest'}`
            return 'Add guests'
        },
        whichSearch() {
            const where = this.$route.path.split('/')[1]
            return where === 'stay' ? false : true
        }
    },
    components: {
        searchIcon
    }
}
</script>
  