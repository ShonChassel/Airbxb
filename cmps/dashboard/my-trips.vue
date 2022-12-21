<template>
    <div v-if="user && trips && currTrip" class="trips-container">
        <!-- <h2>Hi {{ user.fullname }}, you have {{ 2 }} pending trips</h2> -->
        <trip-list @openToDisplay="setTripDisplay" :trips="trips" />
        <div class="trip-display-container">
            <section class="trip-display">
                <section class="trip-img">
                    <img v-for="img in currTrip?.stay.imgUrls" :src="img" alt="" />
                </section>
                <section class="trip-info">
                    <h2>{{ currTrip.stay.name }}</h2>
                    <h3>{{ currTrip?.stay?.loc?.address }}</h3>
                    <p>
                        <span class="info-parameter">Dates:</span>
                        {{ formattedStartDate }} - {{ formattedEndDate }}
                    </p>
                    <p>
                        <span class="info-parameter">Guests:</span>
                        {{ currTrip?.guests?.adults }}
                    </p>
                    <p>
                        <span class="info-parameter">Total price:</span> ${{
                                currTrip?.totalPrice
                        }}
                    </p>
                    <p class="last" :class="tripStatus">
                        <span class="info-parameter">Order Confirmation:</span>
                        {{ currTrip._id }}
                    </p>
                    <p class="last" :class="tripStatus">
                        <span class="info-parameter">Order Status:</span>
                        {{ currTrip.status }}
                    </p>
                </section>
            </section>
        </div>
    </div>
    <section v-else class="trips-else">
        <main class="trips-wrapper">
            <h3>You have 0 trips</h3>
            <button @click="navigate" @mousemove="hoverEffect">
                Look for beautiful place to travel
            </button>
        </main>
    </section>
</template>

<script>
import { userService } from "../../services/user-service.js";

import tripList from "./trip-list.vue";

export default {
    async created() {
        this.user = this.$store.getters.loggedinUser;
        this.trips = await userService.getTripsByUserId(this.user._id);
        this.trips = this.trips.reverse()
        if (!this.trips) return;
        this.currTrip = this.trips[0];
        socketService.on("trip-about-you", (updatedTrip) => {
            const idx = this.trips.findIndex(trip => trip._id === updatedTrip._id)
            this.trips.splice(idx, 1, updatedTrip)
        });
    },
    data() {
        return {
            currTrip: "",
            trips: null,
            user: null,
        };
    },
    components: {
        tripList,
    },
    methods: {
        setTripDisplay(trip) {
            this.currTrip = trip;
        },
        hoverEffect(ev) {
            const button = ev.target;
            const { x, y } = button.getBoundingClientRect();
            button.style.setProperty("--x", ev.clientX - x + "px");
            button.style.setProperty("--y", ev.clientY - y + "px");
        },
        navigate() {
            this.$router.push("/");
        },
    },
    computed: {
        getLoggedinUser() {
            return this.$store.getters.loggedinUser;
        },
        formattedStartDate() {
            let date = new Date(this.currTrip.startDate).toLocaleDateString(
                "en-us",
                { month: "short", day: "numeric" }
            );
            if (date === "Invalid Date") return "";
            return date;
        },
        formattedEndDate() {
            let date = new Date(this.currTrip.endDate).toLocaleDateString(
                "en-us",
                {
                    month: "short",
                    day: "numeric",
                }
            );
            if (date === "Invalid Date") return "";
            return date;
        },
        tripStatus() {
            return this.currTrip.status;
        },
    },
};
</script>
