<template>
    <carousel class="carousel" :settings="settings" :breakpoints="breakpoints">
        <slide v-for="slide in btns" :key="slide">

            <div class="slide-wrapper" @click="setSort(slide.key)">
                <div class="img-wrapper" :class="{selected: isSelected === slide.key}">
                    <img class="img-btn" :src="slide.url" alt="" width="24" height="24" />
                    <div class="imgKey">
                        <span>{{ slide.key }}</span>
                    </div>
                </div>
            </div>

        </slide>

        <template #addons>
            <div class="filter-navigation">
                <navigation :slideWidth="100" />
            </div>
        </template>
    </carousel>
</template>

<script>
// If you are using PurgeCSS, make sure to whitelist the carousel CSS classes
import "vue3-carousel/dist/carousel.css";
import { Carousel, Slide, Navigation } from "vue3-carousel";

export default {
    props: {
        btns: {
            type: Array,
        },
    },
    components: {
        Carousel,
        Slide,
        Navigation,
    },
    data() {
        return {
            isSelected: false,
            filterBy: {
                type: '',
            },
            settings: {
                itemsToShow: 4,
                itemsToScroll: 4,
                snapAlign: "center",
            },
            breakpoints: {
                // 700px and up
                700: {
                    itemsToShow: 7,
                    itemsToScroll: 4,
                    snapAlign: "center",
                },
                // 1024 and up
                1024: {
                    itemsToShow: 13,
                    itemsToScroll: 4,
                    snapAlign: "center",
                },
            },
        }
    },
    computed: {
    },
    methods: {
        setSort(type) {
            this.filterBy.type = type
            this.isSelected = type
            this.$emit("filtered", this.filterBy);
        },
    },
};
</script>
