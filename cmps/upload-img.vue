<template>
    <section :style="background" class="img-upload" :class="{ 'drag-zone': isDragover }" @drop.prevent="handleFile"
        @dragover.prevent="isDragover = true" @dragleave="isDragover = false">
        <label v-if="!isLoading" :class="{ drag: isDragover }">
            <p v-if="!this.img">Upload Image</p>
            <input type="file" @change="handleFile" hidden />
        </label>
    </section>
</template>

<script>
import { uploadImg } from '../services/upload-service.js'

export default {
    name: 'img-upload',
    props: {
        img: String,
        idx: Number,
    },
    data() {
        return {
            isLoading: false,
            isDragover: false,
            imgUrls: ['', '', '', '', ''],
        }
    },
    methods: {
        handleFile(ev) {
            let file
            if (ev.type === 'change') file = ev.target.files[0]
            else if (ev.type === 'drop') file = ev.dataTransfer.files[0]
            this.onUploadFile(file)
        },
        async onUploadFile(file) {
            this.isLoading = true
            this.isDragover = false
            const res = await uploadImg(file)
            this.$emit('setImg', { url: res.url, idx: this.idx })
            this.isLoading = false
        },
    },
    computed: {
        background() {
            return {
                'background-image': `url(${this.img})`,
                'background-repeat': 'no-repeat',
                'background-size': 'cover',

            }
        }
    }
}
</script>
