<script setup>
import { computed } from "vue";
const props = defineProps({
    text: String
})
const emit = defineEmits(["searchText"]);
const text = computed({
    get: () => props.text,
    set: (val) => {
        // 通知父層更新
        emit('searchText', val);
    }
});
</script>
<template>
    <p>
        站點名稱搜尋: <input class="border" type="text" v-model="text">
    </p>
</template>