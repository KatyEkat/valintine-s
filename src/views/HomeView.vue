<script setup lang="ts">
import { ref, computed, nextTick, watch } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const noClickCount = ref(0);
const noButtonPosition = ref<{ left: string; top: string } | null>(null);
const noButtonRef = ref<HTMLButtonElement | null>(null);
const yesStep = ref(0);

function getRandomPosition() {
	const left = 5 + Math.random() * 70;
	const top = 15 + Math.random() * 60;
	return { left: `${left}%`, top: `${top}%` };
}

function getCurrentButtonCenter() {
	const el = noButtonRef.value;
	if (!el) return getRandomPosition();
	const rect = el.getBoundingClientRect();
	const left = rect.left + rect.width / 2;
	const top = rect.top + rect.height / 2;
	return { left: `${left}px`, top: `${top}px` };
}

function onYesClick() {
	if (yesStep.value < 2) {
		yesStep.value += 1;
	} else {
		yesStep.value = 3;
		successImgFailed.value = false;
	}
}

function onNoClick() {
	if (yesStep.value >= 1) {
		router.push("/no");
		return;
	}
	noClickCount.value += 1;
	if (noClickCount.value <= 3) {
		if (noClickCount.value === 1) {
			noButtonPosition.value = getCurrentButtonCenter();
			nextTick().then(() => {
				requestAnimationFrame(() => {
					requestAnimationFrame(() => {
						noButtonPosition.value = getRandomPosition();
					});
				});
			});
		} else {
			noButtonPosition.value = getRandomPosition();
		}
	} else {
		router.push("/no");
	}
}

const noButtonStyle = computed(() =>
	noButtonPosition.value
		? {
				position: "fixed" as const,
				left: noButtonPosition.value.left,
				top: noButtonPosition.value.top,
				transform: "translate(-50%, -50%)",
			}
		: undefined,
);

const stepTitle = computed(() => {
	if (yesStep.value === 0) return "Будешь моим валентином?";
	if (yesStep.value === 1) return "Ты уверен в этом?";
	if (yesStep.value === 2) return "Ты точно уверен?";
	return "";
});

const showConfirmButtons = computed(
	() => yesStep.value >= 1 && yesStep.value <= 2,
);
const showInitialButtons = computed(() => yesStep.value === 0);
const showSuccess = computed(() => yesStep.value === 3);
const successImgFailed = ref(false);
watch(showSuccess, (v) => {
	if (v) successImgFailed.value = false;
});
</script>

<template>
	<main class="home">
		<div v-if="showSuccess" class="home-success">
			<h1 class="home-success__title">лютюдю тебя Сашулька ❤️</h1>
			<h2>Ты наша Ohana</h2>

			<div
				class="home-success__photo"
				:class="{ 'home-success__photo--fallback': successImgFailed }"
			>
				<img
					src="/happy.jpeg"
					alt="Фото"
					class="home-success__img"
					@error="successImgFailed = true"
				/>
			</div>
		</div>

		<template v-else>
			<p class="home-title">{{ stepTitle }}</p>

			<div v-if="showInitialButtons" class="home-buttons">
				<button class="home-button" @click="onYesClick">Да</button>
				<button
					ref="noButtonRef"
					class="home-button home-button--no"
					:style="noButtonStyle"
					@click="onNoClick"
				>
					Нет
				</button>
			</div>

			<div v-if="showConfirmButtons" class="home-buttons">
				<button class="home-button" @click="onYesClick">Да</button>
				<button class="home-button" @click="onNoClick">Нет</button>
			</div>
		</template>
	</main>
</template>

<style scoped>
.home {
	position: relative;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	height: 100vh;
}

.home-title {
	font-size: 1.75rem;
	font-weight: 600;
	color: #1a1a1a;
	margin-bottom: 0.5rem;
}

.home-buttons {
	display: flex;
	gap: 1rem;
}

.home-button {
	padding: 0.5rem 1rem;
	border-radius: 0.25rem;
	border: 1px solid #1a1a1a;
	background-color: #fff;
	color: #1a1a1a;
	font-size: 1rem;
	font-weight: 600;
	cursor: pointer;
	transition:
		background-color 0.2s ease,
		color 0.2s ease;
}
.home-button:hover {
	background-color: rgb(245, 141, 242);
	color: #fff;
}

.home-button--no {
	position: relative;
	transition:
		left 0.45s cubic-bezier(0.34, 1.56, 0.64, 1),
		top 0.45s cubic-bezier(0.34, 1.56, 0.64, 1),
		background-color 0.2s ease,
		color 0.2s ease;
}

.home-success {
	display: flex;
	flex-direction: column;
	align-items: center;
	text-align: center;
}
.home-success__title {
	font-size: 1.75rem;
	font-weight: 600;
	color: #1a1a1a;
	margin-bottom: 1.5rem;
}
.home-success__photo {
	position: relative;
	max-width: 400px;
	min-height: 400px;
	border-radius: 0.5rem;
	overflow: hidden;
	box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
	background-color: #fce4ec;
	background-size: cover;
	background-position: center;
}
.home-success__photo--fallback {
	background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='400' height='400' viewBox='0 0 400 400'%3E%3Crect fill='%23fce4ec' width='400' height='400'/%3E%3Ctext x='50%25' y='50%25' dominant-baseline='middle' text-anchor='middle' fill='%23c2185b' font-size='48'%3E%E2%9D%A4%EF%B8%8F%3C/text%3E%3C/svg%3E");
}
.home-success__photo--fallback .home-success__img {
	opacity: 0;
	position: absolute;
}
.home-success__img {
	width: 100%;
	height: auto;
	display: block;
	object-fit: cover;
}
</style>
