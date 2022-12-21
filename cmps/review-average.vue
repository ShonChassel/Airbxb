<template>
    <div class="review-totals">
        <star></star>
        <span>{{ averageRate.toFixed(1) }}</span> ·
        <a href="#reviews">{{ totalReviews }}</a>
    </div>
</template>

<script>
import star from "../assets/svg/star.vue";

export default {
    props: {
        reviews: Number,
        rateMap: Object,
    },
    computed: {
        totalReviews() {
            if (this.reviews === 1) return "1 review";
            if (this.reviews > 1) return `${this.reviews} reviews`;
            if (!this.reviews) return `No reviews yet...`;
        },
        averageRate() {
            const rateMap = this.rateMap;
            if(rateMap === 'new') return 0
            var sum = 0;
            for (const key in rateMap) {
                sum += rateMap[key];
            }
            return sum / 6;
        },
    },
    components: {
        star,
    },
};
</script>
