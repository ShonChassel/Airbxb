<template>
    <section class="reserve-modal">
        <form @submit.prevent="addOrder">
            <header>
                <h4>
                    <span>{{ formattedPerNightPrice }}</span> night
                </h4>
                <review-average
                    :reviews="stay.reviews.length"
                    :rateMap="rateMap"
                />
            </header>
            <div class="picker-container">
                <div @click="showDatePicker = true" class="check-in picker">
                    <label for="check-in">CHECK-IN</label>
                    <input
                        type="text"
                        placeholder="MM/DD/YYYY"
                        :value="formatedStartDate"
                    />
                </div>
                <section class="date-picker-container">
                    <el-date-picker
                        :teleported="false"
                        @change="setDates"
                        v-model="dates"
                        popper-class="date-picker-reserve"
                        type="daterange"
                        start-placeholder="Start date"
                        end-placeholder="End date"
                    />
                </section>
                <div @click="showDatePicker = true" class="check-out picker">
                    <label for="check-out">CHECK-OUT</label>
                    <input
                        type="text"
                        placeholder="MM/DD/YYYY"
                        :value="formatedEndDate"
                    />
                </div>
                <div
                    @click="this.showGuestPicker = !this.showGuestPicker"
                    class="guests picker"
                >
                    <label for="guests">GUESTS</label>
                    <input
                        type="text"
                        :value="guestsCount"
                        style="color: black; background-color: white"
                        disabled
                    />
                    <img
                        v-if="!showGuestPicker"
                        src="../assets/svg/arrow-down.svg"
                        class="arrow-img"
                    />
                    <img
                        v-else
                        src="../assets/svg/arrow-up.svg"
                        class="arrow-img"
                    />
                    <guests-modal
                        @setGuests="setGuests"
                        :class="{ open: showGuestPicker }"
                    ></guests-modal>
                </div>
            </div>
            <button @mousemove="hoverEffect" class="btn-reserve">
                Reserve
            </button>

            <div
                style="display: flex; gap: 25px; flex-direction: column"
                v-if="order.startDate && order.endDate"
            >
                <p style="text-align: center">You won't be charged yet</p>
                <div class="prices">
                    <p>
                        {{ formattedPerNightPrice }} x {{ getTotalDays }} nights
                    </p>
                    <p>{{ formattedTotalNightsPrice }}</p>
                    <p>Cleaning fee</p>
                    <p>{{ formattedCleaningFee }}</p>
                    <p>Service fee</p>
                    <p>{{ formattedServiceFee }}</p>
                </div>
                <div class="total">
                    <p>Total</p>
                    <p>{{ formattedTotal }}</p>
                </div>
            </div>
        </form>
    </section>
</template>

<script>
import { orderService } from "../services/order-service.js";

import reviewAverage from "../cmps/review-average.vue";
import reactiveBtn from "./reactive-btn.vue";
import guestsModal from "./guests-modal.vue";
import { eventBus } from "../services/event-bus-service";

export default {
    props: {
        stay: Object,
        reviews: Number,
        rateMap: Object,
    },
    data() {
        return {
            dates: [],
            order: null,
            cleaningFee: 0,
            serviceFee: 0,
            amenitiesSrc: [],
            showDatePicker: false,
            showGuestPicker: false,
        };
    },
    async created() {
        this.order = orderService.getEmptyOrder();
        eventBus.on("reserveOrder", this.addOrder);
    },
    methods: {
        setDates() {
            this.order.startDate = this.dates["0"].getTime();
            this.order.endDate = this.dates["1"].getTime();
        },
        setGuests(guests) {
            this.order.guests = { ...guests };
        },
        addOrder() {
            (this.order.stay.loc.address = this.stay.loc.address),
                (this.order.totalPrice =
                    this.serviceFee +
                    this.cleaningFee +
                    this.totalDays() * this.stay.price);
            this.$emit("addOrder", this.order);
        },
        totalDays() {
            const date1 = new Date(this.order.startDate);
            const date2 = new Date(this.order.endDate);
            const diffTime = Math.abs(date2 - date1);
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
            this.serviceFee = diffDays * this.stay.price * 0.17;
            this.cleaningFee = diffDays * this.stay.price * 0.1;
            return diffDays;
        },
        format(num) {
            const formatter = new Intl.NumberFormat("en-US", {
                style: "currency",
                currency: "USD",
            });
            let price = formatter.format(num);
            let dotIdx = price.indexOf(".");
            return price[dotIdx + 1] === "0" ? price.slice(0, dotIdx) : price;
        },
        formatDate(date) {
            const DATE = new Date(date);
            return `${
                DATE.getMonth() + 1
            }/${DATE.getDate()}/${DATE.getFullYear()}`;
        },
        hoverEffect(ev) {
            const button = ev.target;
            const { x, y } = button.getBoundingClientRect();
            button.style.setProperty("--x", ev.clientX - x + "px");
            button.style.setProperty("--y", ev.clientY - y + "px");
        },
    },
    computed: {
        guestsCount() {
            const { adults, children, infants, pets } = this.order.guests;
            let str =
                adults || children
                    ? `${adults + children} ${
                          adults + children !== 1 ? "guests" : "guest"
                      }`
                    : "";
            str += infants
                ? `, ${infants} ${infants !== 1 ? "infants" : "infant"}`
                : "";
            str += pets ? `, ${pets} ${pets !== 1 ? "pets" : "pet"}` : "";
            return str;
        },
        formatedStartDate() {
            return this.formatDate(this.order.startDate);
        },
        formatedEndDate() {
            return this.formatDate(this.order.endDate);
        },
        totalReviews() {
            const { reviews } = this.stay;
            if (reviews.length === 1) return "1 review";
            if (reviews.length > 1) return `${reviews.length} reviews`;
            if (!reviews.length) return `No reviews yet...`;
        },
        formattedPerNightPrice() {
            return this.format(this.stay.price);
        },
        formattedTotalNightsPrice() {
            return this.format(this.totalDays() * this.stay.price);
        },
        isOrderComplete() {
            return this.$store.state.isOrderComplete;
        },
        getTotalDays() {
            return this.totalDays();
        },
        formattedCleaningFee() {
            return this.format(this.cleaningFee);
        },
        formattedServiceFee() {
            return this.format(this.serviceFee);
        },
        formattedTotal() {
            return this.format(
                this.serviceFee +
                    this.cleaningFee +
                    this.totalDays() * this.stay.price
            );
        },
    },
    components: {
        reviewAverage,
        reactiveBtn,
        guestsModal,
    },
};
</script>
