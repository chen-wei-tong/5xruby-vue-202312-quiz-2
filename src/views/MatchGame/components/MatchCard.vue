<script setup>
import { watch } from "vue";
const props = defineProps(["cards", "openedCard"]);
const emit = defineEmits(["clickHandler"]);
const clickHandler = (val) => {
  emit("clickHandler", val);
  console.log(val);
  // if (newidx = oldidx){
  //   console.log("yes")
  // }
};
// watch(clickHandler(idx), (newValue, oldValue) => {
//   console.log("newValue", newValue, oldValue);

// });
// const clickHandler = computed({
//   get: () => props.cards,
//   set: (idx) => {
//     // 通知父層更新
//     emit("clickHandler", idx);
//   },
// });
</script>
<template>
  {{ props.cards }}<br />
  {{ props.openedCard }}<br />
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

<style scoped src="../MatchGame.css"></style>