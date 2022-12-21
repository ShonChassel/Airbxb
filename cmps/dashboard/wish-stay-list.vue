<template>
    <div class="wish-stay-list">
        <wish-stay-preview v-for="wishStay in wishStays" :wishStay="wishStay" />
    </div>
</template>
  
<script>
import wishStayPreview from "./wish-stay-preview.vue";

export default {
    props: {
        wishStays: Array,
    },
    components: {
        wishStayPreview,
    },
};
</script>