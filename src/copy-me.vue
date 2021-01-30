<template>
  <div class="copyWrapper" @click="handleClick()">
    <slot name="handler"></slot>

    <div
      v-if="toastMessage"
      ref="toast"
      class="toast"
      :class="'--' + toastPosition"
      :style="toastCss"
    >
      {{ toastMessage }}
    </div>
  </div>
</template>

<script>
export default {
  name: "CopyMe",
  props: {
    text: {
      type: String,
      required: true,
    },
    notify: {
      type: Boolean,
      default: true,
    },
    successMessage: {
      type: String,
      default: "Copied",
    },
    failMessage: {
      type: String,
      default: "Copied",
    },
    toastCss: {
      type: String,
    },
    toastPosition: {
      type: String,
      default: "up",
    },
  },
  data() {
    return {
      toastMessage: null,
    };
  },

  methods: {
    async handleClick() {
      if(this.toastMessage) return;

      if (navigator.clipboard) {
        await navigator.clipboard.writeText(this.text);
        this.toast("success");
        this.$emit("success");
      } else {
        this.toast("fail");
        this.$emit("fail");
      }
    },

    toast(type) {
      if (this.notify) {
        this.toastMessage = null;
        this.toastMessage = this[type + "Message"];
        setTimeout(() => (this.toastMessage = null), 1000);
      }
    },
  },
};
</script>

<style scoped>
.copyWrapper {
  position: relative;
  display: inline-block;
}

.copyWrapper .toast {
  position: absolute;
  height: 1.3rem;
  width: 5rem;
  background-color: #373737;
  color: #ffffff;
  border-radius: 0.25rem;
  font-size: 0.9rem;
  text-align: center;
  line-height: 1.3rem;
  font-family: "Courier New", Courier, monospace;
  padding: 0.15rem 0.05rem;
}

.toast::after {
  content: " ";
  position: absolute;
  top: 100%;
  left: 50%;
  margin-left: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: #373737 transparent transparent transparent;
}

.--up {
  top: -10px;
  left: 50%;
  transform: translate(-50%, -110%);

  animation-name: d-u;
  animation-duration: 0.15s;
  animation-direction: alternate;
  animation-timing-function: ease-in;
}


@keyframes d-u {
  from {
    top: 15px;
    opacity: 0;
  }
  to {
    top: -10px;
    opacity: 1;
  }
}
</style>
