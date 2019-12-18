<template>
  <div>
    <transition-group
      name="shuffle-items"
      tag="div"
      class="malla"
      appear
      appear-active-class="ficha-enter-active"
    >
      <div v-for="item in items" :key="item.id" class="item">
        <ficha :item="item" @click="select(item)"></ficha>
      </div>
    </transition-group>
  </div>
</template>

<script>
import Ficha from "../components/Ficha.vue";
import { mapGetters } from "vuex";
export default {
  props: {},
  components: { Ficha },
  data: () => ({}),
  computed: {
    ...mapGetters({
      selectedItems: "getSelectedItems",
      totalSelected: "getTotalSelected",
      flippedItems: "getFlippedItems",
      ongoingGame: "getOngoingGame",
      itemsLeft: "getItemsLeft",
      items: "getItems",
      win: "getWin"
    })
  },
  methods: {
    select(item) {
      if (!this.ongoingGame) this.$store.dispatch("setOngoingGame", true);
      if (item.flipped || item.selected || this.totalSelected >= 2) {
        return;
      }
      if ((prevItem = this.selectedItems.find(i => i.value == item.value))) {
        var prevItem;
        this.$store.dispatch("setItemsFlipped", [prevItem, item]);
      }
      this.$store.dispatch("setSelected", item);
    },
    newGame() {
      this.$store.dispatch("newGame");
    }
  },
  created() {},
  mounted() {},
  watch: {
    totalSelected(val) {
      if (val >= 2) {
        this.$store.dispatch("incrementAttempts");
        setTimeout(() => {
          this.$store.dispatch("unselectItemsSelected");
        }, 1000);
      }
    },
    itemsLeft(val) {
      if (val == 0) {
        this.$store.dispatch("setWin", true);
        this.$store.dispatch("setOngoingGame", false);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.malla {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  max-width: 800px;
  margin: 15px;
}

.item {
  flex: 1 0 20%;
  margin-left: 10px;
  margin-right: 10px;
  margin-top: 20px;
  margin-bottom: 20px;
  will-change: transform;
}

.shuffle-items-move {
  transition: transform 0.6s;
}

.ficha-enter-active {
  opacity: 0;
  animation: enter-logo 1000ms;
}

@for $i from 1 through 12 {
  .ficha-enter-active:nth-child(#{$i}n) {
    animation-delay: #{$i * 0.2}s;
  }
}

@keyframes enter-logo {
  from {
    opacity: 0;
    transform: translateY(100%);
    // position: absolute;
  }
  to {
    opacity: 1;
    transform: translateY(0%);
    // position: absolute;
  }
}
</style>
