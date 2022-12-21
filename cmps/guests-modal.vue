<template>
    <section @click.stop class="guests-modal">
        <ul>
            <li>
                <div class="guest-info">
                    <p>Adults</p>
                    <p>Ages 13 or above</p>
                </div>
                <div class="guest-adjust">
                    <button class="adjust-minus" @click.stop.prevent="setGuests('adults', -1)"
                        :disabled="guests.adults === 0"><minus-icon /></button>
                    <div class="guests-value">{{ guests.adults }}</div>
                    <button class="adjust-plus" @click.stop.prevent="setGuests('adults', 1)"
                        :disabled="guests.adults === 10"><plus-icon /></button>
                </div>
            </li>
            <li>
                <div class="guest-info">
                    <p>Children</p>
                    <p>Ages 2-12</p>
                </div>
                <div class="guest-adjust">
                    <button class="adjust-minus" @click.stop.prevent="setGuests('children', -1)"
                        :disabled="guests.children === 0"><minus-icon /></button>
                    <div class="guests-value">{{ guests.children }}</div>
                    <button class="adjust-plus" @click.stop.prevent="setGuests('children', 1)"
                        :disabled="guests.children === 10"><plus-icon /></button>
                </div>
            </li>
            <li>
                <div class="guest-info">
                    <p>Infants</p>
                    <p>Under 2</p>
                </div>
                <div class="guest-adjust">
                    <button class="adjust-minus" @click.stop.prevent="setGuests('infants', -1)"
                        :disabled="guests.infants === 0"><minus-icon /></button>
                    <div class="guests-value">{{ guests.infants }}</div>
                    <button class="adjust-plus" @click.stop.prevent="setGuests('infants', 1)"
                        :disabled="guests.infants === 10"><plus-icon /></button>
                </div>
            </li>
            <li>
                <div class="guest-info">
                    <p>Pets</p>
                    <p>Bringing a service animal?</p>
                </div>
                <div class="guest-adjust">
                    <button class="adjust-minus" @click.stop.prevent="setGuests('pets', -1)"
                        :disabled="guests.pets === 0"><minus-icon /></button>
                    <div class="guests-value">{{ guests.pets }}</div>
                    <button class="adjust-plus" @click.stop.prevent="setGuests('pets',1)"
                        :disabled="guests.pets === 10"><plus-icon /></button>
                </div>
            </li>
        </ul>
    </section>
</template>
  
<script>
import minusIcon from "../assets/svg/minus.vue";
import plusIcon from "../assets/svg/plus.vue";

export default {
    data() {
        return {
            guests: {
                adults: 0,
                children: 0,
                infants: 0,
                pets: 0
            }
        }
    },
    methods: {
        setGuests(key, val) {
            this.guests[key] += val
            this.$emit('setGuests', this.guests)
        }
    },
    components: {
        minusIcon,
        plusIcon
    }
};
</script>
  