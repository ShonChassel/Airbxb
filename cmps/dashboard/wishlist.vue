<template>
    <div v-if="user" class="wishlist-container">
        <wish-stay-list :wishStays="wishlist" />
    </div>
</template>

<script>
import wishStayList from './wish-stay-list.vue';

export default {

    data(){
        return {
            wishlist: null,
            user: null
        }
    },
    created(){
        this.user = this.$store.getters.loggedinUser
        this.wishlist = this.user.wishlist
    },
    components: {
        wishStayList
    }
}
</script>