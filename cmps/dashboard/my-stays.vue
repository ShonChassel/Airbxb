<template>
    <section v-if="user && stays && stays.length" class="my-stay-container">
        <my-stays-list @deleteStay="deleteStay" :stays="stays" />
    </section>
    <section v-else class="my-stay-else">
        <main class="stay-wrapper">
            <h3>You have 0 stays</h3>
            <button @click="navigate" @mousemove="hoverEffect">
                Become a host now
            </button>
        </main>
    </section>
</template>

<script>
import { userService } from "../../services/user-service.js";
import myStaysList from "./my-stay-list.vue";

export default {

    async created() {
        this.user = this.$store.getters.loggedinUser
        this.stays = await userService.getStaysByUserId(this.user._id);
        if (!this.stays) return;
    },
    data() {
        return {
            stays: null,
            user: null
        };
    },
    methods: {
        hoverEffect(ev) {
            const button = ev.target;
            const { x, y } = button.getBoundingClientRect();
            button.style.setProperty("--x", ev.clientX - x + "px");
            button.style.setProperty("--y", ev.clientY - y + "px");
        },
        navigate() {
            this.$router.push("/stay/edit");
        },
        async deleteStay(stayId) {
            try {
                await this.$store.dispatch({ type: "removeStay", stayId });
                await this.$store.dispatch({
                    type: "removeFromUserStays",
                    stayId,
                });
                this.stays = await userService.getStaysByUserId(this.user._id);

            } catch {
                console.log("cant remove stay");
            }
        },
    },
    computed: {},
    components: {
        myStaysList,
    },
};
</script>
