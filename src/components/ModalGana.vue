<template>
  <transition name="win">
    <div v-show="win" class="win-message">
      <div class="message">
        <div style="display: flex;">
          <img
            style="width: 80px"
            src="@/assets/icons/confeti.svg"
            alt="logo"
          />

          <p>Felicitaciones!</p>

          <img
            style="width: 80px"
            src="@/assets/icons/fuegos-artificiales.svg"
            alt="logo"
          />
        </div>
        <button @click="newGame">Nuevo Juego</button>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    win: Boolean
  },
  methods: {
    newGame() {
      this.$store.dispatch("newGame");
    }
  }
};
</script>

<style lang="scss" scoped>
$transition: 0.6s;
$delay: 1.5s;
.win-message {
  position: fixed;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
  z-index: 9;
  background-color: rgba(255, 255, 255, 0.5);
  backdrop-filter: blur(15px);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  will-change: opacity, background-color;
  font-size: 30pt;
}
.message {
  max-width: 100%;
  opacity: 1;
  text-align: center;
  padding: 20px;
  border-radius: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  will-change: backdrop-filter, transform, opacity;
}
.win-enter-active {
  transition: all $transition $delay;
}
.win-leave-active {
  transition: all $transition;
}
.win-enter,
.win-leave-to {
  opacity: 0;
}
.win-enter-active .message {
  transition: all ($transition) ($delay);
}
.win-enter .message {
  opacity: 0;
  transform: scale(0.8);
}
.win-leave-active .message {
  transition: all $transition;
}
.win-leave-to .message {
  opacity: 0;
  transform: scale(0.8);
}
</style>
