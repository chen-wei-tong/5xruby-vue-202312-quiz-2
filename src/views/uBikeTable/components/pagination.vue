<script setup>
import { ref, computed, defineProps, defineEmits } from "vue";
const props = defineProps([
  "pagerEnd",
  "currentPage",
  "uBikeTablePage",
  "totalPageCount",
]);

const emit = defineEmits(["setPage"]);
// 目前頁碼
// const currentPage = ref(1);
// 一頁幾筆資料
const COUNT_OF_PAGE = 20;
// 頁碼最多顯示幾頁
const PAGINATION_MAX = 10;

// 分頁的位移，用來確保目前的頁碼固定出現在中間
const pagerAddAmount = computed(() => {
  console.log(
    "totalPageCount:",
    props.totalPageCount,
    "pagerEnd:",
    pagerEnd.value
  );
  console.log("currentPage", props.currentPage);
  const tmp =
    props.totalPageCount <= PAGINATION_MAX
      ? 0
      : props.currentPage + 4 - pagerEnd.value;
  return tmp <= 0
    ? 0
    : props.totalPageCount - (PAGINATION_MAX + tmp) < 0
    ? props.totalPageCount - PAGINATION_MAX
    : tmp;
});

// 分頁的尾端
const pagerEnd = computed(() => {
  return props.totalPageCount <= PAGINATION_MAX
    ? props.totalPageCount
    : PAGINATION_MAX;
});

// 換頁
const setPage = (page) => {
  if (page < 1 || page > props.totalPageCount) {
    return;
  }
  //   props.currentPage = page;
  emit("setPage", page);
};
</script>
<template>
  <nav v-if="pagerEnd > 0">
    <ul class="pagination">
      <li @click.prevent="setPage(1)" class="page-item">
        <a class="page-link" href>第一頁</a>
      </li>
      <li @click.prevent="setPage(props.currentPage - 1)" class="page-item">
        <a class="page-link" href>&lt;</a>
      </li>

      <li
        v-for="i in pagerEnd"
        :class="{ active: i + pagerAddAmount === props.currentPage }"
        :key="i"
        @click.prevent="setPage(i + pagerAddAmount)"
        class="page-item"
      >
        <a class="page-link" href>{{ i + pagerAddAmount }}</a>
      </li>

      <li @click.prevent="setPage(props.currentPage + 1)" class="page-item">
        <a class="page-link" href>&gt;</a>
      </li>
      <li @click.prevent="setPage(props.totalPageCount)" class="page-item">
        <a class="page-link" href>最末頁</a>
      </li>
    </ul>
  </nav>
</template>