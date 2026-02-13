<script setup lang="ts">
import { RouterView } from "vue-router";

const emojis = ["❤️", "😍", "🥰"];
const floats = Array.from({ length: 18 }, (_, i) => ({
	emoji: emojis[i % emojis.length],
	left: 5 + ((i * 17) % 85),
	top: 10 + ((i * 23) % 75),
	size: ["sm", "md", "lg"][i % 3],
	delay: (i * 0.7) % 4,
	duration: 8 + (i % 5),
}));
</script>

<template>
	<div class="app">
		<div class="app__floats" aria-hidden="true">
			<span
				v-for="(f, i) in floats"
				:key="i"
				:class="['app__float', `app__float--${f.size}`]"
				:style="{
					left: f.left + '%',
					top: f.top + '%',
					animationDelay: f.delay + 's',
					animationDuration: f.duration + 's',
				}"
			>
				{{ f.emoji }}
			</span>
		</div>
		<div class="app__content">
			<RouterView />
		</div>
	</div>
</template>

<style scoped>
.app {
	min-height: 100vh;
	position: relative;
	overflow-x: hidden;
}

.app__floats {
	position: fixed;
	inset: 0;
	pointer-events: none;
	z-index: 0;
	overflow: hidden;
}

.app__float {
	position: absolute;
	opacity: 0.35;
	animation: float ease-in-out infinite;
	will-change: transform;
}

.app__float--sm {
	font-size: 1.25rem;
}
.app__float--md {
	font-size: 1.75rem;
}
.app__float--lg {
	font-size: 2.5rem;
}

@keyframes float {
	0%,
	100% {
		transform: translate(0, 0) scale(1);
	}
	25% {
		transform: translate(6px, -8px) scale(1.05);
	}
	50% {
		transform: translate(-4px, 4px) scale(0.98);
	}
	75% {
		transform: translate(8px, 4px) scale(1.02);
	}
}

.app__content {
	position: relative;
	z-index: 1;
	min-height: 100vh;
}
</style>
