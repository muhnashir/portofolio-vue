<script setup lang="ts">

import {nextTick, onMounted, ref} from "vue";
import {faFilesPinwheel, faHouzz, faSearchengin, faUbuntu} from "@fortawesome/free-brands-svg-icons";
import {library} from "@fortawesome/fontawesome-svg-core";
import NameLabel from "@/components/NameLabel.vue";
import Ascii from "@/components/Ascii.vue";

library.add(faUbuntu, faHouzz,faFilesPinwheel,faSearchengin);

const isShowBannerFirst = ref(true)
const isShowBannerSecond = ref(false)
const inputCommand = ref("")
const commandHistory = ref<string[]>([]);
const inputRef = ref<HTMLInputElement | null>(null);
const result = ref<string[]>([]);
const containerRef = ref(null);

const executeCommand = async () => {
    if (inputCommand.value.trim() === "") return;

    const command = inputCommand.value.toLowerCase();

    if (command === "clear") {
        commandHistory.value = [];
        result.value = [];
        inputCommand.value = "";
        isShowBannerFirst.value = false;
        return;
    }

    if (command === "banner") {
        isShowBannerSecond.value = true;
    }

    commandHistory.value.push(inputCommand.value);
    result.value.push(commands(inputCommand.value));

    inputCommand.value = "";
    await nextTick();
    inputRef.value?.focus();

    containerRef.value?.scrollIntoView({ behavior: "smooth", block: "end" });
};


const commands = (type:string): string => {
    switch (type.toLowerCase()) {
        case "whois" :
            return whois.join("\n");
        case "projects":
                return projects.join("\n");
        case "banner" :
            return "";
        case "clear" :
            return "clear";
        case "help" :
            return help.join("\n");
        default:
            return `Command ${type} not found`;
    }
}


const help = [
    "<div class='flex w-80'>\n"+
        "<span class='w-24 text-white'>whois</span><span class='text-yellow-400 text-left'>Who is Muhammad Nashir</span>\n" +
    "</div>\n"+
    "<div class='flex w-80'>\n"+
        "<span class='w-24 text-white'>projects</span><span class='text-yellow-400 text-left'>View Project</span>\n" +
    "</div>\n"+
    "<div class='flex w-80'>\n"+
        "<span class='w-24 text-white'>help</span><span class='text-yellow-400 text-left'>Show a list Available Commands</span>\n" +
    "</div>\n"+
    "<div class='flex w-80'>\n"+
        "<span class='w-24 text-white'>clear</span><span class='text-yellow-400 text-left'>Clear Terminal</span>\n" +
    "</div>\n"+
    "<div class='flex w-80'>\n"+
        "<span class='w-24 text-white'>banner</span><span class='text-yellow-400 text-left'>Display the Header</span>\n"+
    "</div>"
]

const whois = [
    "HEHEHEHEHE"
]

const projects = [
    "JOS"
]

</script>

<template>
  <div class="w-full min-h-screen bg-gray-800 text-green-300 shadow-lg md:p-6 lg:p-4 overflow-x-auto md:px-0 animate-fade-in">

    <Ascii :isShowBanner="isShowBannerFirst"></Ascii>

    <div id="input">
      <div class="mt-2">
          <div v-if="commandHistory.length > 0">
              <div v-for="(cmd, index) in commandHistory" :key="index" class="text-yellow-400">
                  <NameLabel></NameLabel>
                  <div class="mt-2">
                      ⚡ Nashir<span class="text-cyan-300"> >> </span> <span class="text-white ml-1">{{ cmd }}</span>
                      <div class="text-white text-sm ml-2" v-if="result.length > 0" v-html="result[index].replace(/\n/g, '')"></div>
                      <Ascii v-if="result[index] === ''" :isShowBanner="true" />
                  </div>
              </div>
          </div>
      </div>

      <div class="mt-2 text-yellow-400" ref="containerRef">
          <NameLabel></NameLabel>
        <div class="flex w-full mt-2">
          <div>
            ⚡ Nashir<span class="text-cyan-300"> >> </span>
          </div>
          <div class="relative flex items-center flex-grow ml-2">
            <input
                type="text"
                v-model="inputCommand"
                class="bg-transparent outline-none text-white"
                :size="inputCommand.length || 1"
                @keyup.enter="executeCommand"
                autofocus
            />
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<style>

@keyframes blinker {
    50% {
        opacity: 0;
    }
}
@keyframes fade-in {
    from {
        opacity: 0;
        transform: translateY(-20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.animate-fade-in {
    animation: fade-in 0.5s ease-out;
}
</style>