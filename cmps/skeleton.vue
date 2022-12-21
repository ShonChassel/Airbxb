<template>
    <article class="stay-preview" style="display: flex; flex-direction: column; gap: 10px;">
        <div class="img-skeleton skeleton"></div>
        <!-- <img class="skeleton" src="######" alt="" /> -->
        <div style="display: flex; flex-direction: column; gap: 2px;">
            <div style="display: flex; justify-content: space-between;">
                <div class="skeleton skeleton-txt skeleton-name"></div>
                <div class="skeleton skeleton-txt skeleton-rate"></div>
            </div>
            <div class="skeleton skeleton-txt skeleton-desc"></div>
            <div class="skeleton skeleton-txt skeleton-date"></div>
        </div>
        <div class="skeleton skeleton-txt skeleton-price"></div>
    </article>
</template>

<script>
export default {
    props: {
        stay: Object,
    },
};
</script>
