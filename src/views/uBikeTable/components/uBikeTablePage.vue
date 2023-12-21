<script setup>
import { ref, watch, computed } from "vue";
const props = defineProps([
  "text",
  "currentSort",
  "isSortDesc",
  "filtedUbikeStops",
  "slicedUbikeStops",
]);
const emit = defineEmits(["setSort", "keywordsHighlight"]);

// 指定排序
const setSort = (sortType) => {
  if (sortType === props.currentSort) {
    props.isSortDesc = !props.isSortDesc;
  } else {
    props.currentSort = sortType;
    props.isSortDesc = false;
  }
  console.log("currentSort", props.currentSort);
  console.log("isSortDesc", props.isSortDesc);
  emit("setSort", sortType);
};

// 關鍵字 Highlight
const keywordsHighlight = (text, keyword) => {
  if (keyword === "") return text;
  const reg = new RegExp(keyword, "gi");
  return text.replace(reg, `<span style="color: red;">${keyword}</span>`);
};

//   get: () => {
//     return (sortType) => {
//       console.log("text", props.text);
//       if (sortType === currentSort.value) {
//         isSortDesc.value = !isSortDesc.value;
//         console.log(isSortDesc.value);
//       } else {
//         currentSort.value = sortType;
//         isSortDesc.value = false;
//         console.log(currentSort.value);
//       }
//     };
//   },
//   set: (val) => {
//     sortedUbikeStops.value = val;
//     console.log(sortedUbikeStops.value);
//     console.log("sortedUbikeStops", "filtedUbikeStops", val);
//   },
// });

// 篩選後的站點資料
// const filtedUbikeStops = computed(() => {
//   return uBikeStops.value.length === 0
//     ? []
//     : uBikeStops.value.filter((d) => d.sna.includes(text.value));
// });

// 排序後的站點資料
// const sortedUbikeStops = computed(() => {
//   const filtedStops = [...filtedUbikeStops.value];
//   return isSortDesc.value
//     ? filtedStops.sort((a, b) => b[props.currentSort] - a[props.currentSort])
//     : filtedStops.sort((a, b) => a[props.currentSort] - b[props.currentSort]);
// });

// 分頁後的站點資料
// const slicedUbikeStops = computed(() => {
//   const start = (currentPage.value - 1) * COUNT_OF_PAGE;
//   const end =
//     start + COUNT_OF_PAGE <= sortedUbikeStops.value.length
//       ? start + COUNT_OF_PAGE
//       : sortedUbikeStops.value.length;
//   return sortedUbikeStops.value.slice(start, end);
// });
</script>
<template>
  <table class="table table-striped">
    <thead>
      <tr>
        <th @click="setSort('sno')">
          #
          <span v-show="props.currentSort === 'sno'">
            <i
              class="fa"
              :class="isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'"
              aria-hidden="true"
            ></i>
          </span>
        </th>
        <th>場站名稱</th>
        <th>場站區域</th>
        <th @click="setSort('sbi')" class="pointer">
          目前可用車輛
          <span v-show="props.currentSort === 'sbi'">
            <i
              class="fa"
              :class="props.isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'"
              aria-hidden="true"
            ></i>
          </span>
        </th>
        <th @click="setSort('tot')" class="pointer">
          總停車格
          <span v-show="props.currentSort === 'tot'">
            <i
              class="fa"
              :class="props.isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'"
              aria-hidden="true"
            ></i>
          </span>
        </th>
        <th>資料更新時間</th>
      </tr>
    </thead>
    <tbody>
      <!-- 替換成 slicedUbikeStops -->
      <tr v-for="s in slicedUbikeStops" :key="s.sno">
        <td>{{ s.sno }}</td>
        <td v-html="keywordsHighlight(s.sna, props.text)"></td>
        <td>{{ s.sarea }}</td>
        <td>{{ s.sbi }}</td>
        <td>{{ s.tot }}</td>
        <td>{{ s.mday }}</td>
      </tr>
    </tbody>
  </table>
</template>