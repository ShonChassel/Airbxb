<template>
    <div class="review-preview">
        <div class="mini-user-details">
            <img loading="lazy" :src="review.by.imgUrl" alt="" />
            <p>{{ review.by.fullname }}</p>
            <span>{{ formattedDate }}</span>
        </div>
        <p class="review-text" ref="reviewText" :class="`text${idx}`">{{ review.txt }}</p>
        <button v-if="isMoreThan3" @click="showMore">Show more</button>
    </div>
</template>

<script>
export default {
    props: {
        review: Object,
        idx: Number,
    },
    data() {
        return {
            isShowMore: true,
            isMoreThan3: false
        };
    },
    created() {
        setTimeout(this.moreThanThreeLines, 1)
    },
    methods: {
        showMore(ev) {
            ev.target.innerText = `Show ${this.isShowMore ? "less" : "more"}`;
            document.querySelector(`.text${this.idx}`).style.display = this
                .isShowMore
                ? "block"
                : "-webkit-box";
            this.isShowMore = !this.isShowMore;
        },
        moreThanThreeLines() {
            const elP = this.$refs.reviewText
            if (elP.clientHeight > 72) {
                elP.classList.add('long-text')
                this.isMoreThan3 = true
            }
        }
    },
    computed: {
        formattedDate() {
            let date = new Date(this.review.at).toLocaleDateString("en-us", {
                month: "short",
                year: "numeric",
            });
            return date === "Invalid Date" ? "" : date;
        },

    },
};
</script>
