<template>
  <div class="stay-list">
    <stay-preview v-for="stay in stays" :stay="stay" />
  </div>
</template>

<script>
import stayPreview from "./stay-preview.vue" 

export default {
  props: {
    stays: {
      type: Array,
    },
  },
  components: {
    stayPreview,
  },
} 
</script>
