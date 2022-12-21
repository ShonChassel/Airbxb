<template>
    <section class="trip-preview">
        <div class="">
            <h4>{{ trip.stay.name }}</h4>
            <p>
                <span class="address">{{ trip.stay.loc.address }}</span>
                <span>{{ formattedStartDate }} - {{ formattedEndedDate }}</span>
            </p>
        </div>
        <span class="trip-status" :class="tripStatus">
            <i v-if="trip.status === 'approve'" class="fa fa-check"></i>
            <i v-if="trip.status === 'decline'" class="fa fa-times-circle"></i>
            <span v-if="trip.status === 'pending'" class='fa Clock-Rotate-Left fontawesomeicon'></span>
            {{ trip.status }}
        </span>
    </section>
</template>

<script>
export default {
    props: {
        trip: Object,
    },
    computed: {
        status() {
            return this.status;
        },
        formattedPrice() {
            const formatter = new Intl.NumberFormat("en-US", {
                style: "currency",
                currency: "USD",
                maximumFractionDigits: 0,
            });
            return formatter.format(this.trip.totalPrice);
        },
        formattedStartDate() {
            let date = new Date(this.trip.startDate).toLocaleDateString(
                "en-us",
                { month: "short", day: "numeric" }
            );
            if (date === "Invalid Date") return "";
            return date;
        },
        formattedEndedDate() {
            let date = new Date(this.trip.endDate).toLocaleDateString(
                "en-us",
                { month: "short", day: "numeric" }
            );
            if (date === "Invalid Date") return "";
            return date;
        },
        tripStatus() {
            return this.trip.status
        }
    },
};
</script>
