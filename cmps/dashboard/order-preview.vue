<template>
    <section class="order-preview list-preview-all">
        <p class="img-buyer"><img :src="order.buyer.imgUrl" alt="" />{{ order.buyer.fullname }}</p>
        <p>{{ order.stay.name }}</p>
        <p>{{ formattedStartDate }}</p>
        <p>{{ order.totalPrice }}$</p>
        <p :class="orderStatus">
            <i class="fa" :class="getIconClass"></i>
            {{ order.status }}
        </p>
        <!-- <div class="actions">
            <button v-if="order.status !== 'approve'" class="approve"
                @click="$emit('changeStatus', 'approve', order._id)">
                <i class="fa fa-check"></i>
                Approve
            </button>
            <button v-if="order.status !== 'decline'" class="decline"
                @click="$emit('changeStatus', 'decline', order._id)">
                Decline
            </button>
        </div> -->
        <el-select @change="$emit('changeStatus', $event, this.order._id)" class="m-2" placeholder="Select"
            size="large">
            <el-option label="Approve" value="approve" />
            <el-option label="Decline" value="decline" />
            <!-- <el-option label="Pending" value="pending" /> -->
        </el-select>
    </section>
</template>

<script>
export default {
    props: {
        order: Object,
    },
    computed: {
        formattedStartDate() {
            return this.formatDate(this.order.startDate);
        },
        formattedEndDate() {
            return this.formatDate(this.order.endDate);
        },
        orderStatus() {
            return this.order.status;
        },
        getIconClass() {
            if (this.order.status === 'pending')
                return 'Clock-Rotate-Left'
            else if (this.order.status === 'approve')
                return 'fa-check'
            else return 'fa-times-circle'
        }
    },
    methods: {
        formatDate(date) {
            const DATE = new Date(date);
            return `${DATE.getMonth() + 1
                }/${DATE.getDate()}/${DATE.getFullYear()}`;
        },
    },
};
</script>
