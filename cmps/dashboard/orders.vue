<template>
    <div v-if="orders && orders.length" class="order-container">
        <div class="order-header">
            <h2>Hi {{ userFullname }}, you have {{ orders.length }} orders </h2>
            <h3><span><i class="fa fa-solid fa-circle"></i>{{ pendingLength }}
                    pending </span><span><i class="fa fa-solid fa-circle"></i>{{ approveLength }}
                    approved </span><span><i class="fa fa-solid fa-circle"></i>{{ declineLength }} declined</span>

            </h3>
        </div>
        <order-list @changeStatus="updateStatus" :orders="orders" />
    </div>
    <section v-else class="my-stay-else">
        <main class="stay-wrapper">
            <h3>No orders yet...</h3>
            <!-- <button @click="navigate" @mousemove="hoverEffect">
                Become a host now
            </button> -->
        </main>
    </section>
</template>

<script>
import { orderService } from '../../services/order-service';
import { userService } from '../../services/user-service.js'
import { socketService } from '../../services/socket-service.js'

import orderList from './order-list.vue';

export default {
    data() {
        return {
            orders: null,
            user: null,
        }
    },
    async created() {
        this.user = this.$store.getters.loggedinUser
        this.orders = await userService.getOrdersByUserId(this.user._id);
        socketService.on('order-about-you', order => {
            this.orders.unshift(order)
        })
        this.orders = this.orders.reverse()
    },
    methods: {
        async updateStatus(val, id) {
            let order = await orderService.getById(id)
            order.status = val
            await this.$store.dispatch({ type: 'addOrder', order })
            this.orders = await userService.getOrdersByUserId(this.user._id)
            this.orders = this.orders.reverse()
        },
        calcLength(status) {
            return this.orders.filter(order => order.status === status).length
        }
    },
    computed: {
        userFullname() {
            return `${this.user.fullname[0].toUpperCase()}${this.user.fullname.slice(1, this.user.fullname.length)}`
        },
        pendingLength() {
            return this.calcLength('pending')
        },
        approveLength() {
            return this.calcLength('approve')
        },
        declineLength() {
            return this.calcLength('decline')
        }
    },
    components: {
        orderList
    }
}
</script>
