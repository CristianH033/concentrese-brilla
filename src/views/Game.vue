<template>
  <div id="app" class="game">
    <transition name="win">
      <div v-show="win" class="win-message">
        <div class="message">
          <img
            style="width: 150px"
            src="@/assets/icons/fuegos-artificiales.svg"
            alt="logo"
          />
          <p>Felicitaciones!</p>
          <button @click="newGame">Nuevo Juego</button>
        </div>
      </div>
    </transition>
    <div class="logo-pad">
      <p>Intentos: {{ attempts }}</p>
      <transition appear appear-active-class="logo-enter-active">
        <img class="logo" src="@/assets/svg/logo.svg" alt="logo" />
      </transition>
    </div>
    <div class="pad">
      <malla></malla>
      <div class="info">
        <!-- <button @click="winGame">Ganar Juego</button> -->
        <button @click="atras">Inicio</button>
        <button @click="newGame">Nuevo Juego</button>
      </div>
    </div>
  </div>
</template>

<script>
import Malla from "../components/Malla.vue";
import { mapGetters } from "vuex";
export default {
  props: {},
  components: { Malla },
  data: () => ({}),
  computed: {
    ...mapGetters({
      attempts: "getAttempts",
      win: "getWin",
      onGoingGame: "getOngoingGame"
    })
  },
  methods: {
    newGame() {
      this.$store.dispatch("newGame");
    },
    winGame() {
      this.$store.dispatch("setItemsFlipped", this.$store.getters.getItems);
    },
    atras() {
      this.$router.go(-1);
    }
  },
  created() {},
  mounted() {},
  watch: {}
};
</script>

<style lang="scss" scoped>
$transition: 0.5s;
$delay: 1s;

.game {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0px;
  left: 0px;
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  justify-content: space-around;
}
.win-message {
  position: fixed;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
  z-index: 9;
  background-color: rgba(255, 255, 255, 0.7);
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
  box-shadow: 0px 3px 10px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(10px);
  padding: 20px;
  border-radius: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  will-change: backdrop-filter, transform, opacity;
}
.logo-pad {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: flex-start;
}
.logo {
  // flex-grow: 1;
  align-self: flex-end;
  margin: 15px;
  max-width: 300px;
  width: 100%;
}

.logo-enter-active {
  animation: enter-logo 1s;
}

@keyframes enter-logo {
  from {
    opacity: 0;
    transform: translateY(-100%);
  }
  to {
    opacity: 1;
    transform: translateY(0%);
  }
}

.pad {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}
.info {
  display: flex;
  align-items: center;
  justify-content: space-between;
  align-items: center;
  font-size: 20pt;
}

// Transitions
.win-enter-active {
  transition: all $transition $delay;
}
.win-leave-active {
  transition: all $transition;
}
.win-enter,
.win-leave-to {
  background-color: rgba(255, 255, 255, 0);
}

.win-enter-active .message {
  transition: transform ($transition) ($delay);
}
.win-enter .message {
  // opacity: 0;
  // transform: scale(1.1);
  transform: translateY(calc(50vh + 50%));
}

.win-leave-active .message {
  transition: transform $transition;
}
.win-leave-to .message {
  // opacity: 0;
  // transform: scale(1.1);
  transform: translateY(calc(50vh + 50%));
}
</style>
