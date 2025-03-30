<template>
    <div :class="[
        'cursor-pointer select-none p-2 hover:bg-accent rounded-md transition-colors duration-200 flex items-center justify-center',
    ]" @mouseenter="handleMouseEnter" @mouseleave="handleMouseLeave">
        <svg xmlns="http://www.w3.org/2000/svg" :width="size" :height="size" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
            <motion.path d="M5.8 11.3 2 22l10.7-3.79" :variants="popperVariants" :animate="currentState" />
            <motion.path
                d="M11 13c1.93 1.93 2.83 4.17 2 5-.83.83-3.07-.07-5-2-1.93-1.93-2.83-4.17-2-5 .83-.83 3.07.07 5 2Z"
                :variants="popperVariants" :animate="currentState" />
            <motion.circle cx="4" cy="3" r="0.5" :variants="dotsVariants" :animate="currentState" />
            <motion.circle cx="22" cy="8" r="0.5" :variants="dotsVariants" :animate="currentState" />
            <motion.circle cx="15" cy="2" r="0.5" :variants="dotsVariants" :animate="currentState" />
            <motion.circle cx="22" cy="20" r="0.5" :variants="dotsVariants" :animate="currentState" />

            <motion.path
                d="m14 10 1.21-1.06c0.16-0.84 0.9-1.44 1.76-1.44h0.38c0.88 0 1.55-0.77 1.45-1.63a2.9 2.9 0 0 1 1.96-3.12L22 2"
                :variants="linesVariants" :animate="currentState" />
            <motion.path d="M17 15h0.77c0.71 0 1.32-0.52 1.43-1.22c0.16-0.91 1.12-1.45 1.98-1.11L22 13"
                :variants="linesVariants" :animate="currentState" />
            <motion.path d="M9 7V6.23c0-0.71 0.52-1.33 1.22-1.43c0.91-0.16 1.45-1.12 1.11-1.98L11 2"
                :variants="linesVariants" :animate="currentState" />
        </svg>
    </div>
</template>
<script setup lang="ts">
import { motion } from "motion-v";

interface Props {
    size?: number;
    class?: string;
}

const props = withDefaults(defineProps<Props>(), {
    size: 28,
    class: "",
});

const emit = defineEmits<{
    startAnimation: [];
    stopAnimation: [];
}>();

const linesVariants = {
    normal: {
        opacity: 1,
        pathLength: 1,
        scale: 1,
        translateX: 0,
        translateY: 0,
    },
    animate: {
        opacity: [0, 0.99],
        scale: [0.5, 1],
        pathLength: [0, 1],
        translateX: [-5, 0],
        translateY: [5, 0],
        transition: {
            type: "spring",
            damping: 35,
            duration: 0.7,
            stiffness: 240,
            velocity: 0.3,
        },
    },
};

const dotsVariants = {
    normal: { opacity: 1, scale: 1, translateX: 0, translateY: 0 },
    animate: {
        translateX: [-1.5, 0],
        translateY: [1.5, 0],
        transition: {
            type: "spring",
            damping: 35,
            stiffness: 200,
            velocity: 0.3,
            mass: 4,
        },
    },
};

const popperVariants = {
    normal: { opacity: 1, scale: 1, translateX: 0, translateY: 0 },
    animate: {
        translateX: [-1.5, 0],
        translateY: [1.5, 0],
        transition: {
            type: "spring",
            damping: 35,
            stiffness: 200,
            velocity: 0.3,
            mass: 4,
        },
    },
};

const isControlled = ref(false);
const currentState = ref("normal");

const startAnimation = () => {
    currentState.value = "animate";
};

const stopAnimation = () => {
    currentState.value = "normal";
};

const handleMouseEnter = () => {
    if (!isControlled.value) {
        startAnimation();
    } else {
        emit("startAnimation");
    }
};

const handleMouseLeave = () => {
    if (!isControlled.value) {
        stopAnimation();
    } else {
        emit("stopAnimation");
    }
};

defineExpose({
    startAnimation,
    stopAnimation,
});
</script>
