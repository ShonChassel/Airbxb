<template>
    <section :class="{scrolled: isScrolled}" class="stay-filter main-container full">
        <section
            class="main-container"
            style="display: flex; align-items: center"
        >
            <filter-btns :btns="btnsAryy()" @filtered="setFilterBy" />

            <button @click="toggleFilterModal(true)" class="btn-filters">
                <div>
                    <img src="../assets/svg/filter-btn.svg" alt="" />
                    <p>Filters</p>
                </div>
            </button>
        </section>
    </section>
    <filter-modal
        :stayPrices="prices"
        @close="toggleFilterModal(false)"
        v-if="isFilterOpen"
    />
</template>

<script>
import { stayService } from "../services/stay-service.js";

import filterBtns from "./filter-btns.vue";
import filterModal from "./filter-modal.vue";

export default {
    props: {
        stays: Array,
    },
    data(){
        return {
            isScrolled: false
        }
    },
    created() {
        window.addEventListener("scroll", this.func);
    },
    computed: {
        isFilterOpen() {
            return this.$store.getters.isFilterOpen;
        },
        prices() {
            const prices = this.stays.map((stay) => stay.price + "");
            return prices;
        },
    },
    methods: {
        func() {
            if (window.scrollY > 0) {
                this.isScrolled = true
            } else {
                this.isScrolled = false
                
            }
        },
        toggleFilterModal(isShown) {
            this.$store.commit({ type: "toggleFilterModal", bool: isShown });
        },
        setFilterBy(filterBy) {
            this.$store.dispatch({ type: "setFilter", filterBy });
        },
        btnsAryy() {
            return stayService.btnsAryy();
        },
    },
    components: {
        filterBtns,
        filterModal,
    },
};
</script>
