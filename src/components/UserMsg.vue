<template>
  <div v-if="alive" class="alert" :class="alertClass">
    {{ msg.txt }}
  </div>
</template>
<script>
import { eventBusService } from "../services/event-bus.service";

export default {
  created() {
    eventBusService.on("show-msg", this.showMsg);
  },
  data() {
    return {
      alive: false,
      msg: { txt: "" },
      timeout: 0,
    };
  },
  methods: {
    showMsg(msg) {
      var delay = msg.delay || 2000;
      this.msg = msg;
      this.alive = true;

      clearTimeout(this.timeout);
      this.timeout = setTimeout(() => (this.alive = false), delay);
    },
  },
  computed: {
    alertClass() {
      if (!this.msg) return;
      return `alert-${this.msg.type}`;
    },
  },
};
</script>
