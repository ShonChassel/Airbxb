<template>
    <section class="destination-modal">
        <section class="common-search">
            <p>Common search</p>
            <ul>
                <li @click="setDestination('Qatar')">
                    <div class="spot-holder"><spot-icon /></div> Qatar, QA
                </li>
                <li @click="setDestination('Canada')">
                    <div class="spot-holder"><spot-icon /></div> Canada, CA
                </li>
                <li @click="setDestination('Portugal')">
                    <div class="spot-holder"><spot-icon /></div> Portugal, PT
                </li>
                <li @click="setDestination('Spain')">
                    <div class="spot-holder"><spot-icon /></div> Spain, ES
                </li>
                <li @click="setDestination('Turkey')">
                    <div class="spot-holder"><spot-icon /></div> Turkey, TR
                </li>
            </ul>
        </section>
        <section class="regions">
            <p>Search by region</p>
            <div @click="setDestination('Flex')">
                <img src="https://a0.muscache.com/pictures/f9ec8a23-ed44-420b-83e5-10ff1f071a13.jpg" alt="Flexible">
                Flexible
            </div>
            <div @click="setDestination('Europe')">
                <img src="https://a0.muscache.com/im/pictures/7b5cf816-6c16-49f8-99e5-cbc4adfd97e2.jpg?im_w=320"
                    alt="Europe">
                Europe
            </div>
            <div @click="setDestination('France')">
                <img src="https://a0.muscache.com/im/pictures/f0ece7c0-d9b2-49d5-bb83-64173d29cbe3.jpg?im_w=320"
                    alt="France">
                France
            </div>
            <div @click="setDestination('United States')">
                <img src="https://a0.muscache.com/im/pictures/4e762891-75a3-4fe1-b73a-cd7e673ba915.jpg?im_w=320"
                    alt="United States">
                United States
            </div>
            <div @click="setDestination('United Kingdom')">
                <img src="https://a0.muscache.com/im/pictures/dbb2b5ef-2efe-4099-81ac-c7b957f384ed.jpg?im_w=320"
                    alt="United Kingdom">
                United Kingdom
            </div>
            <div @click="setDestination('South America')">
                <img src="https://a0.muscache.com/im/pictures/06a30699-aead-492e-ad08-33ec0b383399.jpg?im_w=320"
                    alt="South America">
                South America
            </div>
        </section>
    </section>
</template>

<script>
import spotIcon from '../assets/svg/spot.vue'

export default {
    methods: {
        setDestination(val) {
            this.$emit('setDestination', val)
        }
    },
    components: {
        spotIcon
    }
}
</script>

<style lang="scss" scoped>

</style>