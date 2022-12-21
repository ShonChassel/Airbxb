<template>
    <div class="full">
        <header ref="elDetailsHeader" :class="{ sticky: sticky }" class="details-header full secondary-container">
            <section class="secondary-container">
                <div class="reserve-container">
                    <div class="reserve-info-container">
                        <p>{{ formattedPrice }} <span>night</span></p>
                        <review-average :reviews="reviews" :rateMap="rateMap" />
                    </div>
                    <button @mousemove="hoverEffect" @click="addOrder" class="btn-reserve">
                        Reserve
                    </button>
                </div>
            </section>
        </header>
    </div>
</template>

<script>
import reviewAverage from "../cmps/review-average.vue";
import { eventBus } from "../services/event-bus-service";

export default {
    props: {
        stay: Object,
        sticky: Boolean,
        showReserve: Boolean,
        reviews: Number,
        rateMap: Object,
    },
    methods: {
        hoverEffect(ev) {
            const button = ev.target;
            const { x, y } = button.getBoundingClientRect();
            button.style.setProperty("--x", ev.clientX - x + "px");
            button.style.setProperty("--y", ev.clientY - y + "px");
        },
        addOrder() {
            eventBus.emit('reserveOrder')
        }
    },
    computed: {
        formattedPrice() {
            const formatter = new Intl.NumberFormat('en-US', {
                style: 'currency',
                currency: 'USD',
                maximumFractionDigits: 0,
            })
            let num = Number(formatter.format(this.stay.price))
            return formatter.format(this.stay.price)
        },
    },
    components: {
        reviewAverage,
    },
};
</script>
