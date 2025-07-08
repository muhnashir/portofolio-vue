<script setup lang="ts">
import {ref, defineProps, defineEmits} from "vue";

const asciiArt = [
    "               ,---------------------------,",
    "               |  /---------------------\\  |",
    "               | |                       | |",
    "               | |       Hello           | |",
    "               | |       World !!!       | |",
    "               | |                       | |",
    "               | |                       | |",
    "               |  \\_____________________/  |",
    "               |___________________________|",
    "             ,---\\_____     []     _______/------,",
    "           /         /______________\\           /|",
    "         /___________________________________ /  | ___",
    "         |                                   |   |    )",
    "         |  _ _ _                 [-------]  |   |   (",
    "         |  o o o                 [-------]  |  /    _)_",
    "         |__________________________________ |/     /  /",
    "     /-------------------------------------/|      ( )/",
    "   /-/-/-/-/-/-/-/-/-/-/-/-/-/-/-/-/-/-/-/ /",
    " /-/-/-/-/-/-/-/-/-/-/-/-/-/-/-/-/-/-/-/ /",
    " ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~",
]

const asciiArt2 = [
    "  /$$$$$$                               /$$                                   /$$        /$$           /$$ /$$                              \n" +
    " /$$__  $$                             | $$                                  | $/       | $$          |__/| $$                              \n" +
    "| $$  \\ $$  /$$$$$$$ /$$$$$$$  /$$$$$$ | $$  /$$$$$$  /$$$$$$/$$$$  /$$   /$$|_//$$$$$$ | $$  /$$$$$$  /$$| $$   /$$ /$$   /$$ /$$$$$$/$$$$ \n" +
    "| $$$$$$$$ /$$_____//$$_____/ |____  $$| $$ |____  $$| $$_  $$_  $$| $$  | $$  |____  $$| $$ |____  $$| $$| $$  /$$/| $$  | $$| $$_  $$_  $$\n" +
    "| $$__  $$|  $$$$$$|  $$$$$$   /$$$$$$$| $$  /$$$$$$$| $$ \\ $$ \\ $$| $$  | $$   /$$$$$$$| $$  /$$$$$$$| $$| $$$$$$/ | $$  | $$| $$ \\ $$ \\ $$\n" +
    "| $$  | $$ \\____  $$\\____  $$ /$$__  $$| $$ /$$__  $$| $$ | $$ | $$| $$  | $$  /$$__  $$| $$ /$$__  $$| $$| $$_  $$ | $$  | $$| $$ | $$ | $$\n" +
    "| $$  | $$ /$$$$$$$//$$$$$$$/|  $$$$$$$| $$|  $$$$$$$| $$ | $$ | $$|  $$$$$$/ |  $$$$$$$| $$|  $$$$$$$| $$| $$ \\  $$|  $$$$$$/| $$ | $$ | $$\n" +
    "|__/  |__/|_______/|_______/  \\_______/|__/ \\_______/|__/ |__/ |__/ \\______/   \\_______/|__/ \\_______/|__/|__/  \\__/ \\______/ |__/ |__/ |__/\n" +
    "                                                                                                                                            \n"
]

const props = defineProps({
    isShowBanner: Boolean,
});

const emit = defineEmits(['executeCommand']);

const runHelpCommand = () => {
    emit('executeCommand', 'help');
};
</script>

<template>
    <div id="banner" class="py-4 md:py-6 ascii-container" v-if="isShowBanner">
        <pre class="text-green-400 text-[4px] sm:text-sm md:text-lg font-mono leading-tight whitespace-pre md:text-left lg:text-left ascii-art">{{ asciiArt2.join("\n") }}</pre>
        <p class="text-yellow-400 text-xs sm:text-sm md:text-base font-mono pl-2 md:text-left lg:text-left mt-2 welcome-text">
            Welcome to my <span class="text-blue-400 interactive-text">Interactive Web UI</span><br>
            For a list available commands, type <span @click="runHelpCommand" class="text-red-400 command-text">'help'</span>.
        </p>
    </div>
</template>

<style scoped>
.ascii-container {
    transform-style: preserve-3d;
    perspective: 1000px;
}

.ascii-art {
    transform: rotateX(5deg);
    transition: transform 0.5s ease;
    text-shadow: 0 0 5px rgba(0, 255, 0, 0.5);
}

.ascii-art:hover {
    transform: rotateX(0deg);
    text-shadow: 0 0 10px rgba(0, 255, 0, 0.8);
}

.welcome-text {
    opacity: 0;
    animation: fadeIn 1s ease-in forwards 0.5s;
}

.interactive-text {
    position: relative;
    display: inline-block;
    transition: all 0.3s ease;
}

.interactive-text:hover {
    text-shadow: 0 0 10px rgba(0, 150, 255, 0.8);
    transform: scale(1.05);
}

.command-text {
    position: relative;
    display: inline-block;
    transition: all 0.3s ease;
    cursor: pointer;
}

.command-text:hover {
    text-shadow: 0 0 10px rgba(255, 0, 0, 0.8);
    transform: scale(1.05);
}

@keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
}
</style>
