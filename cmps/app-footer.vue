<template>
    <div class="app-footer" :class="whichContainer" :style="whichLayout">
        <section class="wrapper">
            <div class="coffeerights">
                <p>© 2022 Airbxb, inc</p>
                •
                <p>Privacy</p>
                •
                <p>Terms</p>
                •
                <p>Sitemap</p>
            </div>
            <div class="social-media">
                <!-- <a href="">facebook</a> -->
            </div>
        </section>
        
    </div>
</template>
  
<script>
export default {
    computed: {
        whichLayout() {
            const layout = this.$route.path.split("/")[1];
            return layout === "stay" || layout==="dashboard" ? { "position": "static" } : { "position": "fixed" };
        },
        whichContainer() {
            const layout = this.$route.path.split("/")[1];
            return layout === "stay" || layout==="dashboard" ? 'secondary-container': 'main-container';
        },
    },
}
</script>
  