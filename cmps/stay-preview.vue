<template>
    <router-link :to="'stay/' + stay._id" target="_blank">
        <article class="stay-preview">
            <heart-icon
                @click.stop.prevent="setWishlist"
                :class="{ mark: isMark }"
                class="heart-btn"
            />
            <img-carousel @click.prevent :stayId="stay._id" :imgs="stay.imgUrls" />
            <section class="preview-info">
                <p class="location">
                    {{ location
                    }}<span class="rate"
                        ><star-icon />&nbsp; {{ rate }} ({{
                            this.stay.reviews.length
                        }})</span
                    >
                </p>
                <main>
                    <p class="stay-name">{{ stay.name }}</p>
                    <p class="date">{{ date }}</p>
                </main>
                <p class="price"><span class="price-label">{{ formattedPrice }}</span> night</p>
               
            </section>
        </article>
    </router-link>
   


</template>

<script>
import { wishlistMsg } from '../services/event-bus-service.js';

import arrowLeft from "../assets/svg/arrow-left.vue";
import arrowRight from "../assets/svg/arrow-right.vue";
import starIcon from "../assets/svg/star.vue";
import heartIcon from "../assets/svg/heart.vue";
import imgCarousel from "../cmps/img-carousel.vue";

import { utilService } from "../services/utils-service.js";

export default {
    props: {
        stay: Object,
    },
    data() {
        return {
            isMark: false,
        };
    },
    created() {
        const wishlist = this.loggedinUser?.wishlist;
        if (!wishlist) return;
        const idx = wishlist.findIndex((stay) => stay._id === this.stay._id);
        this.isMark = idx > -1 ? true : false;
    },
    methods: {
        async setWishlist() {
            this.isMark = !this.isMark;
            await this.$store.dispatch({
                type: "setWishlist",
                stayId: this.stay._id,
            });
            const msg = `${this.stay.name} ${this.isMark ? `saved to wishlist` : `removed from wishlist`}`
            wishlistMsg(msg);
        },
    },
    computed: {
        date() {
            return utilService.getDates();
        },

        rate() {
            if (this.stay.reviews.length === 0) return "New";
            let sum = this.stay.reviews.reduce((acc, { rate }) => {
                let rating = Object.values(rate);
                let calcRate = Number(
                    (rating.reduce((acc, num) => (acc += num), 0) / 6).toFixed(
                        1
                    )
                );
                acc += calcRate;
                return acc;
            }, 0);
            sum /= this.stay.reviews.length;
            return `${sum.toFixed(1)}`;
        },
        location() {
            return `${this.stay.loc.city}, ${this.stay.loc.country}`;
        },
        distanceFromMe() {
            return this.stay.position;
        },
        formattedPrice() {
            const formatter = new Intl.NumberFormat("en-US", {
                style: "currency",
                currency: "USD",
                maximumFractionDigits: 0,
            });
            let num = Number(formatter.format(this.stay.price));
            return formatter.format(this.stay.price);
        },
        loggedinUser() {
            return this.$store.getters.loggedinUser;
        },
    },
    components: {
        arrowLeft,
        arrowRight,
        starIcon,
        heartIcon,
        imgCarousel,
    },
};
</script>
