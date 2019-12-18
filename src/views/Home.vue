<template>
  <div class="home">
    <img src="@/assets/svg/logo.svg" class="logo" alt />
    <div class="botones">
      <button @click="newGame">Nuevo Juego</button>
      <button v-if="onGoingGame" @click="resumeGame">Reanudar Juego</button>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  props: {},
  data: () => ({}),
  computed: {
    ...mapGetters({
      onGoingGame: "getOngoingGame"
    })
  },
  methods: {
    newGame() {
      this.$router.push({ name: "game" });
      this.$store.dispatch("newGame");
    },
    resumeGame() {
      this.$router.push({ name: "game" });
    }
  },
  created() {},
  mounted() {}
};
</script>

<style lang="scss" scoped>
.home {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0px;
  left: 0px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}
.logo {
  max-width: 400px;
  width: 100%;
}
.botones {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}
</style>
