<template>
    <section class="login-layout">
        <header class="login-title-cont">
            <h2>Log in or sign up</h2>
            <button @click="close" class="exit-login-btn">
                <img src="../assets/svg/close.svg" alt="" />
            </button>
        </header>

        <form @submit.prevent="login" class="login-main-content" v-if="isLogin">
            <h3> Welcome to Airbxb </h3>

            <div class="login-form-cont">
                <input v-model="cred.username" autocomplete="username" placeholder="Username">
                <div class="form-line"></div>
                <input v-model="cred.password" placeholder="Password" type="password" autocomplete="password">
            </div>

            <button @mousemove="hoverEffect" class="btn-container">Log in</button>

            <div class="login-or-line"></div>
            <div class="login-line-text">or</div>

            <button @click="loginAsGuest" @mousemove="hoverEffect" class="btn-container">Continue as a guest</button>

            <div @click="openSignUp()" class="move-to-signup">Don't have an acount yet? sign up</div>
        </form>

        <form @submit.prevent="signup" class="login-main-content" v-if="!isLogin">
            <h3> Welcome to Airbxb </h3>

            <div class="sign-up-form-cont">
                <input v-model="signupCred.fullname" class="fullname-input" type="text" placeholder="Full name">
                <div class="form-line"></div>
                <input v-model="signupCred.username" class="username-input" placeholder="Username">
                <div class="form-line"></div>
                <input v-model="signupCred.password" class="password-input" autocomplete="password"
                    placeholder="Password" type="password">
            </div>

            <button @mousemove="hoverEffect" class="btn-container">Sign up</button>

            <div class="login-or-line"></div>
            <div class="login-line-text">or</div>

            <button @mousemove="hoverEffect" class="btn-container">Continue as a guest</button>

            <div @click="openSignUp()" class="move-to-signup">Already signed up?</div>

        </form>
    </section>
</template>
  
<script>
import { eventBus } from '../services/event-bus-service';
export default {
    name: "login-page",
    data() {
        return {
            isLogin: true,
            cred: {
                username: "",
                password: "",
            },
            signupCred: {
                fullname: "",
                username: "",
                password: "",
            },
        };
    },
    methods: {
        openSignUp() {
            this.isLogin = !this.isLogin;
        },
        async login() {
            await this.$store.dispatch({ type: "login", cred: this.cred });
            this.$emit("closeModal");
            this.$store.commit({ type: "toggleLogInModal", bool: false });
        },
        async signup() {
            await this.$store.dispatch({ type: "signup", cred: this.signupCred });
            this.$emit("closeModal");
            this.$store.commit({ type: "toggleLogInModal", bool: false });
        },
        close() {
            this.$store.commit({ type: "toggleLogInModal", bool: false });
            eventBus.emit('closeLoginModal')
        },
        loginAsGuest() {
            this.cred.username = 'guest'
            this.cred.password = '1'
            this.login()
        }
    },
    computed: {
        users() {
            return this.$store.getters.loggedinUser;
        },
    },
};
</script>
  