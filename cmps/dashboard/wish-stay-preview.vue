<template>
    <div class="wish-stay-preview">
        <router-link :to="('/stay/' + wishStay._id)">
            <div class="imgs-container">
                <img v-for="img in wishStay.imgUrls.slice(0, 3)" :src="img" alt="cool pic of stay">
            </div>
            <h3>{{ wishStay.name }}</h3>
        </router-link>
    </div>
</template>

<script>
export default {
    props: {
        wishStay: Object
    },
}
</script>