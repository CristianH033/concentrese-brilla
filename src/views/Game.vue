<template>
  <div id="app" class="game">
    <modal-gana :win="win" />
    <div class="logo-pad">
      <p>Intentos: {{ attempts }}</p>
      <transition appear appear-active-class="logo-enter-active">
        <img class="logo" src="@/assets/svg/logo.svg" alt="logo" />
      </transition>
    </div>
    <div class="pad">
      <malla></malla>
      <div class="buttons">
        <button @click="atras">Inicio</button>
        <!-- <button @click="winGame">Ganar Juego</button> -->
        <button @click="newGame">Nuevo Juego</button>
      </div>
    </div>
  </div>
</template>

<script>
import Malla from "../components/Malla.vue";
import ModalGana from "../components/ModalGana.vue";
import { mapGetters } from "vuex";
export default {
  props: {},
  components: { Malla, ModalGana },
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
.buttons {
  display: flex;
  align-items: center;
  justify-content: space-between;
  align-items: center;
  font-size: 20pt;
  padding: 15px;
}
</style>
