<template>
    <div>
        <section class="order-preview order-preview-header">
            <p>Buyer</p>
            <p>Stay Name</p>
            <p>Date</p>
            <p>Price</p>
            <p>Status</p>
        </section>
        <div class="order-list ">
            <order-preview @changeStatus="updateStatus" v-for="order in orders" :order="order" />
        </div>
    </div>
</template>
  
<script>
import orderPreview from "./order-preview.vue";

export default {
    props: {
        orders: {
            type: Array,
        },
    },
    methods: {
        updateStatus(val, id) {
            console.log(val, id)
            this.$emit('changeStatus', val, id)
        },
    },
    components: {
        orderPreview,
    },
};
</script>