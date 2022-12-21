<template>
<Transition name="user-msg">
  <div v-if="msg" class="alert" :class="alertClass">
    <i :class="iconClass"></i>
    <p>{{ msg?.txt }}</p>
  </div>
</Transition>
</template>


<script>
import { eventBus, SHOW_MSG } from "../services/event-bus-service.js"

export default {
  created() {
    eventBus.on(SHOW_MSG, (msg) => {
      this.msg = msg
      var delay = msg.delay || 2000
      this.alive = true
      setTimeout(() => {
        this.alive = false
        this.msg = null
      }, delay)
    })
  },
  data() {
    return {
      alive: false,
      msg: null,
    }
  },
  computed: {
    alertClass() {
      if (!this.msg) return
      return `alert-${this.msg.type}`
    },
    iconClass() {
      return this.msg?.icon
    },
  },
}
</script>