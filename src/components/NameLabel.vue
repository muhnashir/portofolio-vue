<script setup lang="ts">

import {faFilesPinwheel, faHouzz, faSearchengin, faUbuntu} from "@fortawesome/free-brands-svg-icons";
import {FontAwesomeIcon} from "@fortawesome/vue-fontawesome";
import {onMounted, ref} from "vue";

const currentTime = ref<string>("");
const updateTime = (): void => {
    const now = new Date();
    currentTime.value = now.toLocaleTimeString();
}

onMounted(()=>{
    updateTime();
    setInterval(updateTime,1000)
});
</script>

<template>
    <div class="flex flex-wrap md:flex-nowrap gap-1 overflow-hidden text-xs sm:text-sm mt-4 px-1">
        <span class="bg-green-700 text-green-300 px-2 py-1 rounded-l-lg">
            <div class="flex items-center justify-center">
                  <FontAwesomeIcon :icon="faUbuntu" class="text-black w-6 h-6" />
                  Nashir
            </div>
        </span>
        <span class="bg-red-600 text-red-300 px-2 py-1">
              <FontAwesomeIcon :icon="faFilesPinwheel" class="text-black w-6 h-6" />
              ➜
              <FontAwesomeIcon :icon="faHouzz" class="text-black w-6 h-6" />
            </span>
        <span class="bg-pink-600 text-white px-2 py-1">/Opt/Dev</span>
        <span class="bg-blue-700 text-blue-300 px-2 py-1">
              {{ currentTime}}
            </span>
        <span class="bg-cyan-700 text-cyan-300 px-2 py-1 rounded-r-lg">
              <FontAwesomeIcon :icon="faSearchengin" class="text-black w-6 h-6" />
            </span>
    </div>
</template>

<style scoped>

</style>