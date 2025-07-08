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
const historyIndex = ref(-1);
const inputRef = ref<HTMLInputElement | null>(null);
const result = ref<string[]>([]);
const containerRef = ref(null);
const isTyping = ref(false);
const availableCommands = ["whois", "projects", "banner", "clear", "help"];

// Navigate through command history using up/down arrow keys
const navigateHistory = (direction: 'up' | 'down') => {
    if (commandHistory.value.length === 0) return;

    if (direction === 'up') {
        historyIndex.value = historyIndex.value < commandHistory.value.length - 1 
            ? historyIndex.value + 1 
            : commandHistory.value.length - 1;
    } else {
        historyIndex.value = historyIndex.value > 0 
            ? historyIndex.value - 1 
            : -1;
    }

    inputCommand.value = historyIndex.value >= 0 
        ? commandHistory.value[commandHistory.value.length - 1 - historyIndex.value] 
        : "";
};

// Auto-complete command when tab is pressed
const autocompleteCommand = () => {
    const input = inputCommand.value.toLowerCase();
    if (!input) return;

    const matchingCommands = availableCommands.filter(cmd => cmd.startsWith(input));
    if (matchingCommands.length === 1) {
        inputCommand.value = matchingCommands[0];
    } else if (matchingCommands.length > 1) {
        // Show available options
        commandHistory.value.push("");
        result.value.push(matchingCommands.join(", "));
    }
};

// Handle command from Ascii component
const handleAsciiCommand = (command: string) => {
    inputCommand.value = command;
    executeCommand();
};

const executeCommand = async () => {
    if (inputCommand.value.trim() === "") return;

    const command = inputCommand.value.toLowerCase();
    historyIndex.value = -1; // Reset history index

    // Visual feedback - disable input during processing
    isTyping.value = true;
    const originalCommand = inputCommand.value;
    inputCommand.value = "";

    // Add command to history
    commandHistory.value.push(originalCommand);

    // Handle clear command specially
    if (command === "clear") {
        commandHistory.value = [];
        result.value = [];
        isShowBannerFirst.value = false;
        isTyping.value = false;
        return;
    }

    if (command === "banner") {
        isShowBannerSecond.value = true;
    }

    // Get command result
    const commandResult = commands(originalCommand);

    // Add result with a slight delay to simulate processing
    setTimeout(() => {
        result.value.push(commandResult);
        isTyping.value = false;

        // Focus back on input and scroll to bottom
        nextTick(() => {
            inputRef.value?.focus();
            containerRef.value?.scrollIntoView({ behavior: "smooth", block: "end" });
        });
    }, 300); // 300ms delay for visual effect
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
    "<div class='text-yellow-400 font-bold text-lg mb-2'>Muhammad Nashir</div>",
    "<div class='mb-4'>",
    "  <p>I am a passionate software developer with expertise in web development and UI/UX design.</p>",
    "  <p>My skills include:</p>",
    "  <ul class='list-disc ml-6 mt-2'>",
    "    <li>Frontend: Vue.js, React, TypeScript, Tailwind CSS</li>",
    "    <li>Backend: Node.js, Express, PHP, Laravel</li>",
    "    <li>Database: MySQL, MongoDB, PostgreSQL</li>",
    "    <li>DevOps: Docker, Git, CI/CD pipelines</li>",
    "  </ul>",
    "</div>",
    "<div class='mt-2'>",
    "  <p>Contact me at: <span class='text-blue-400'>email@example.com</span></p>",
    "  <p>GitHub: <a href='https://github.com/username' class='text-blue-400 hover:underline'>github.com/username</a></p>",
    "  <p>LinkedIn: <a href='https://linkedin.com/in/username' class='text-blue-400 hover:underline'>linkedin.com/in/username</a></p>",
    "</div>"
]

const projects = [
    "<div class='grid grid-cols-1 md:grid-cols-2 gap-4'>",
    "  <div class='bg-gray-700 p-4 rounded-lg'>",
    "    <h3 class='text-yellow-400 font-bold'>Portfolio Website</h3>",
    "    <p class='text-sm'>A terminal-inspired portfolio website built with Vue.js and Tailwind CSS.</p>",
    "    <div class='mt-2 text-xs text-cyan-300'>Vue.js • TypeScript • Tailwind CSS</div>",
    "  </div>",
    "  <div class='bg-gray-700 p-4 rounded-lg'>",
    "    <h3 class='text-yellow-400 font-bold'>E-commerce Platform</h3>",
    "    <p class='text-sm'>A full-stack e-commerce solution with admin dashboard and payment integration.</p>",
    "    <div class='mt-2 text-xs text-cyan-300'>React • Node.js • MongoDB</div>",
    "  </div>",
    "  <div class='bg-gray-700 p-4 rounded-lg'>",
    "    <h3 class='text-yellow-400 font-bold'>Task Management App</h3>",
    "    <p class='text-sm'>A collaborative task management application with real-time updates.</p>",
    "    <div class='mt-2 text-xs text-cyan-300'>Vue.js • Firebase • Vuex</div>",
    "  </div>",
    "  <div class='bg-gray-700 p-4 rounded-lg'>",
    "    <h3 class='text-yellow-400 font-bold'>Weather Dashboard</h3>",
    "    <p class='text-sm'>A weather application that displays forecasts and historical data.</p>",
    "    <div class='mt-2 text-xs text-cyan-300'>JavaScript • Weather API • Chart.js</div>",
    "  </div>",
    "</div>"
]

</script>

<template>
  <div class="w-full min-h-screen bg-gray-800 text-green-300 shadow-lg md:p-6 lg:p-4 overflow-x-auto md:px-0 animate-fade-in">

    <Ascii :isShowBanner="isShowBannerFirst" @executeCommand="handleAsciiCommand"></Ascii>

    <div id="input">
      <div class="mt-2">
          <div v-if="commandHistory.length > 0">
              <div v-for="(cmd, index) in commandHistory" :key="index" class="text-yellow-400 mb-4">
                  <NameLabel></NameLabel>
                  <div class="mt-2">
                      <div class="flex items-center">
                          <span>⚡ Nashir<span class="text-cyan-300"> >> </span></span>
                          <span class="text-white ml-1">{{ cmd }}</span>
                      </div>

                      <!-- Command result -->
                      <div v-if="result.length > index" class="text-white text-sm ml-4 mt-1 command-result">
                          <div v-if="result[index] === ''" class="mt-2">
                              <Ascii :isShowBanner="true" />
                          </div>
                          <div v-else class="typing-effect" v-html="result[index].replace(/\n/g, '')"></div>
                      </div>
                  </div>
              </div>
          </div>
      </div>

      <div class="mt-4 text-yellow-400" ref="containerRef">
          <NameLabel></NameLabel>

          <!-- Loading indicator when processing command -->
          <div v-if="isTyping" class="flex items-center mt-2">
              <span>⚡ Nashir<span class="text-cyan-300"> >> </span></span>
              <span class="ml-2 text-green-400">Processing command...</span>
              <span class="ml-2 loading-dots">...</span>
          </div>

          <!-- Command input -->
          <div v-else class="flex w-full mt-2">
              <div class="flex items-center">
                  ⚡ Nashir<span class="text-cyan-300"> >> </span>
                  <span v-if="!inputCommand" class="h-5 w-2 bg-white cursor-blink inline-block align-middle ml-1"></span>
                  <div v-else class="relative inline-block">
                      <span class="text-white">{{ inputCommand }}</span>
                      <span class="h-5 w-2 bg-white cursor-blink inline-block align-middle ml-0.5"></span>
                  </div>
              </div>
              <input
                  ref="inputRef"
                  type="text"
                  v-model="inputCommand"
                  class="absolute opacity-0 top-0 left-0 w-full h-full"
                  @keyup.enter="executeCommand"
                  @keydown.up="navigateHistory('up')"
                  @keydown.down="navigateHistory('down')"
                  @keydown.tab.prevent="autocompleteCommand"
                  autofocus
              />
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

.cursor-blink {
    animation: blinker 1s step-end infinite;
}

.typing-animation {
    overflow: hidden;
    border-right: 2px solid white;
    white-space: nowrap;
    animation: typing 3.5s steps(40, end), blinker 0.75s step-end infinite;
}

.typing-effect {
    opacity: 0;
    animation: fadeIn 0.5s ease-in forwards;
}

.command-result {
    position: relative;
    padding: 8px;
    border-radius: 4px;
    transition: all 0.3s ease;
}

.command-result:hover {
    background-color: rgba(255, 255, 255, 0.05);
}

.loading-dots {
    display: inline-block;
    position: relative;
    animation: loadingDots 1.5s infinite;
}

@keyframes typing {
    from { width: 0 }
    to { width: 100% }
}

@keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
}

@keyframes loadingDots {
    0% { opacity: 0.2; }
    20% { opacity: 1; }
    100% { opacity: 0.2; }
}
</style>
