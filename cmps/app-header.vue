<template>
    <header class="main-header full" :class="whichLayout">
        <div @click="goToHome" class="logo-container">
            <img src="../assets/img/favicon.png" alt="" />
            <p>airbxb</p>
        </div>
        <stay-search
            :date="currDate"
            :dest="currDest"
            :guests="currGuests"
            @toggleSearch="openZone"
            :class="{ close: isOpen }"
        />
        <search-modal
            @setDate="setDate"
            @setDest="setDest"
            @setGuests="setGuests"
            @updateZone="openZone"
            :zone="zone"
            :class="{ open: isOpen }"
        />
        <!-- <search-modal-mobile /> -->
        <div class="user">
            <a class="become-host" href="/stay/edit">Become a host</a>
            <div class="user-menu-btn" @click="toggleUserMenu()">
                <button>
                    <img class="menu-btn" src="../assets/svg/menu.svg" />
                    <img
                        v-if="!user"
                        class="host-image"
                        src="https://res.cloudinary.com/nisan/image/upload/v1658872030/air2b/unprofile_ji7zus.png"
                    />
                    <img v-else class="host-image" :src="user.imgUrl" />
                </button>
                <userMenu
                    v-outside="closeMenu"
                    v-if="isMenu"
                    @closeMenu="closeMenu"
                />
            </div>
        </div>
    </header>
</template>

<script>
import { eventBus } from "../services/event-bus-service";

import staySearch from "./stay-search.vue";
import searchModal from "./search-modal.vue";
import userMenu from "./user-menu.vue";
import searchModalMobile from './search-modal-mobile.vue'

export default {
    props: {
        close: {
            type: Boolean,
        },
        emptyZone: String,
    },
    data() {
        return {
            logInUser: {},
            isMenu: false,
            currZone: "",
            currDest: "",
            currDate: "",
            currGuests: '',
            isSearch: false,
        };
    },
    created() {
        eventBus.on("closeDatePicker", this.closeDatePicker);
    },
    methods: {
        closeDatePicker() {
            this.currZone = "";
        },
        setDate(str) {
            this.currDate = str;
        },
        setDest(val) {
            this.currDest = val;
        },
        setGuests(val) {
            this.currGuests = val;
        },
        openZone(ev, key) {
            this.currZone = key;
            if (ev.target.innerText === "Any week") this.$emit("clickDate");
            this.$store.commit({ type: "toggleSearch", bool: true });
        },
        toggleUserMenu() {
            this.isMenu = !this.isMenu;
        },
        closeMenu() {
            this.isMenu = false;
        },
        goToHome() {
            this.$router.push("/");
        },
    },
    computed: {
        user() {
            return this.$store.getters.loggedinUser;
        },
        zone() {
            return this.currZone;
        },
        isOpen() {
            return this.$store.getters.open;
        },
        whichLayout() {
            const layout = this.$route.path.split("/")[1];
            return layout === "stay" || layout === "dashboard"
                ? "secondary-container"
                : "main-container";
        },
        currPath(){
            return this.$route.path
        }
    },
    components: {
        staySearch,
        searchModal,
        userMenu,
        searchModalMobile
    },
    watch:{
        currPath(path){
            if(path === '/'){
                this.setDate('')
                this.setDest('')
            }
        }
    }
};
</script>
