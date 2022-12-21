<template>
    <div class="reservation-success">
        <header>
            <h2>Reservation success!</h2>
            <button @click="closeSuccessModal">
                <img src="../assets/svg/close.svg" alt="">
            </button>
        </header>
        <div class="content">
            <div class="your-trip-container">
                <h3>Your trip</h3>
                <div class="info-container">
                    <p>Check-In</p>
                    <p>{{ formatedStartDate }}</p>
                    <p>Check-Out</p>
                    <p>{{ formatedEndDate }}</p>
                </div>
            </div>
            <div class="your-stay-details-container">
                <h3>Stay details</h3>
                <div class="stay-details-info-container">
                    <img :src="stay.imgUrls[0]" alt="">
                    <p>{{ stay.name }}</p>
                    <p>Hosted by:  {{ stay.host.fullname }}</p>
                    <review-average :reviews="stay.reviews.length" :rateMap="rateMap" />
                    <!-- <p>{{ formattedPerNightPrice }}</p> -->
                </div>
            </div>
            <div class="price-details-container">
                <h3>Price details</h3>
                <div class="info-container">
                    <p>Adults</p>
                    <p>{{ order.guests.adults }}</p>
                    <p>Total price</p>
                    <p>{{ formattedTotalPrice }}</p>
                    <p>Total nights</p>
                    <p>{{ getTotalDays }}</p>
                </div>
            </div>
        </div>
        <button @click="goToDashboard" @mousemove="hoverEffect" class="btn-reserve">Check your trips</button>
    </div>
</template>

<script>
import { utilService } from '../services/utils-service.js';
import reviewAverage from './review-average.vue';

export default {
    props: {
        order: Object,
        rateMap: Object,
        stay: Object
    },
    methods: {
        closeSuccessModal() {
            this.$store.commit({ type: "toggleSuccessModal", bool: false });
        },
        totalDays() {
            const date1 = new Date(this.order.startDate);
            const date2 = new Date(this.order.endDate);
            const diffTime = Math.abs(date2 - date1);
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
            return diffDays
        },
        format(num) {
            const formatter = new Intl.NumberFormat('en-US', {
                style: 'currency',
                currency: 'USD',
            });
            return formatter.format(num)
        },
        formatDate(date) {
            const DATE = new Date(date);
            return `${DATE.getMonth() + 1
                }/${DATE.getDate()}/${DATE.getFullYear()}`;
        },
        hoverEffect(ev) {
            utilService.hoverEffect(ev)
        },
        goToDashboard() {
            this.closeSuccessModal()
            this.$router.push("/dashboard/buyer/trips");
        },
    },
    computed: {
        formattedPerNightPrice() {
            return this.format(this.stay.price)
        },
        formattedTotalPrice() {
            return this.format(this.order.totalPrice)
        },
        getTotalDays() {
            return this.totalDays()
        },
        formatedStartDate() {
            return this.formatDate(this.order.startDate);
        },
        formatedEndDate() {
            return this.formatDate(this.order.endDate);
        },
    },
    components: {
        reviewAverage,
    }
}
</script>