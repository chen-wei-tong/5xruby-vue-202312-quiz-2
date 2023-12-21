<script setup>
import { ref, watch } from "vue";
const props = defineProps(["cards", "openedCard"]);
const emit = defineEmits(["clickHandler", "gameInit"]);

const clickHandler = (idx) => {
  emit("clickHandler", idx);
  console.log("clickHandler", idx);
  console.log("props.cards[idx]",props.cards[idx])
};
</script>
<template>
  <div
    v-for="(n, idx) in cards"
    class="flip-card"
    :class="{
      open: openedCard.includes(idx),
    }"
    @click="clickHandler(idx)"
  >
    <div class="flip-card-inner" v-if="cards[idx] > 0">
      <div class="flip-card-front">
        <h1>{{ props.cards[idx] }}</h1>
      </div>
      <div class="flip-card-back">
        <img :src="`./img/cat-0${n}.jpg`" alt="" />
      </div>
    </div>
  </div>
</template>

<style scoped src="../MatchGame.css">
</style>
<style scoped>
.match {
  background: #000;
}
</style>