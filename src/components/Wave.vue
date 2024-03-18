<template>
  <div>
    <p>Assalomu aleykum folks <span class="wave">{{ handEmoji }}</span></p>
  </div>
</template>

<script>
import random from "lodash/random";
const hands = ["👋", "👋🏻", "👋🏼", "👋🏽", "👋🏾", "👋🏿"];
export default {
  data() {
    return {
      currentIndex: random(0, hands.length - 1),
    };
  },

  computed: {
    handEmoji() {
      return hands[this.currentIndex];
    },
  },

  created() {
    setInterval(() => {
      this.currentIndex = random(0, hands.length - 1);
    }, 2000);
  },
};
</script>

<style scoped>
.wave {
  animation-duration: 5s;
  animation-name: waving;
  animation-iteration-count: 1;
  display: inline-block;
  transform-origin: bottom right;
  font-weight: bold;
  font-size: 40px;
}

@keyframes waving {
  from {
    transform: rotate(30deg);
  }

  5% {
    transform: rotate(-10deg);
  }

  10% {
    transform: rotate(30deg);
  }

  15% {
    transform: rotate(-10deg);
  }

  20% {
    transform: rotate(30deg);
  }

  to {
    transform: rotate(0deg);
  }
}
</style>