<template>
  <div
    class="flip-card"
    :class="{ selected: item.selected | item.flipped, flipped: item.flipped }"
    @click="$emit('click')"
  >
    <div class="flip-card-inner">
      <div class="flip-card-front">
        <img class="question" src="@/assets/icons/pregunta.svg" alt="" />
      </div>
      <div
        class="flip-card-back"
        :style="{
          'background-image': `url(${require(`@/assets/svg/${item.value}.svg`)})`
        }"
      >
        <transition name="wrong">
          <img
            v-if="wrong"
            class="wrong"
            src="@/assets/icons/cancelar.svg"
            alt=""
          />
        </transition>
        <img class="right" src="@/assets/icons/comprobado.svg" alt="" />
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  props: { item: Object },
  data: () => ({}),
  computed: {
    ...mapGetters({
      selectedItems: "getSelectedItems"
    }),
    totalSelected() {
      return this.selectedItems.length;
    },
    wrong() {
      return (
        this.item.selected && !this.item.flipped && this.totalSelected >= 2
      );
    }
  },
  methods: {},
  created() {},
  mounted() {}
};
</script>

<style lang="scss" scoped>
.flip-card {
  perspective: 500px;
  width: 120px;
  height: 120px;
  border-radius: 50%;
  cursor: pointer;
  will-change: transform;
}

.flip-card-inner {
  position: relative;
  display: flex;
  width: 100%;
  height: 100%;
  transition: transform 0.5s;
  transform-style: preserve-3d;
  border-radius: 50%;
  will-change: transform;
}

.flip-card.selected .flip-card-inner {
  transform: rotateY(180deg);
}

.flip-card-front {
  background-color: rgb(190, 0, 0);
  color: white;
}
.flip-card-back {
  background-color: rgb(255, 255, 255);
  background-size: 80%;
  background-repeat: no-repeat;
  background-position: 50% 50%;
}
.flip-card-front,
.flip-card-back {
  font-size: 45pt;
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 50%;
  border: 5px solid black;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0px 3px 10px rgba(0, 0, 0, 0.5);
}

.flip-card-front.wrong,
.flip-card-back.wrong {
  border: 5px solid rgb(182, 0, 0);
}

.flip-card-back {
  transform: rotateY(180deg);
}

.flipped {
  animation: match 0.6s 0.4s ease-in-out;
}

img.question,
img.right,
img.wrong {
  width: 100%;
}

.right {
  display: none;
  opacity: 0;
}

.flipped .right {
  display: block;
  animation: check 1.4s 0.6s ease-in-out;
}

.wrong-enter-active,
.wrong-leave-active {
  transition: opacity 0.5s;
  transition-delay: 0.5s;
}
.wrong-enter,
.wrong-leave-to {
  opacity: 0;
}

@keyframes match {
  0% {
    transform: scale(1);
    // background-color: rgb(131, 255, 131);
  }
  25% {
    transform: scale(0.9);
  }
  50% {
    transform: scale(1.2);
  }
  75% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
    // background-color: rgb(131, 255, 131);
  }
}

@keyframes check {
  0% {
    opacity: 0;
  }
  25% {
    opacity: 1;
  }
  75% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
</style>
