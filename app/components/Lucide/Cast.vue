<template>
    <div :class="['cursor-pointer select-none p-2 hover:bg-accent rounded-md transition-colors duration-200 flex items-center justify-center',props.class,]"
        @onMouseEnter="handleMouseEnter" @onMouseLeave="handleMouseLeave">
        <svg xmlns="http://www.w3.org/2000/svg" :width="size" :height="size" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
            <motion.path d="M2 8V6a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v12a2 2 0 0 1-2 2h-6" />
            <motion.path :variants="variants" :animate="currentState" :custom="0.2" d="M2 12a9 9 0 0 1 8 8" />
            <motion.path :variants="variants" :animate="currentState" :custom="0.1" d=" M2 16a5 5 0 0 1 4 4" />
            <motion.line :variants="variants" :custom="0" :animate="currentState" x1="3" x2="2.01" y1="20" y2="20" />
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

const variants =  {
    normal: { opacity: 1 },
    animate: (custom: unknown) => ({
        opacity: [0, 1],
        transition: {
            delay: custom,
            duration: 0.5,
        },
    }),
};

const emit = defineEmits<{
    startAnimation: [];
    stopAnimation: [];
}>();


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
