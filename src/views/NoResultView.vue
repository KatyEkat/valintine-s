<script setup lang="ts">
import { useRouter } from "vue-router";

const router = useRouter();

const placeholder =
	"data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='400' height='500' viewBox='0 0 400 500'%3E%3Crect fill='%23fce4ec' width='400' height='500'/%3E%3Ctext x='50%25' y='50%25' dominant-baseline='middle' text-anchor='middle' fill='%23c2185b' font-size='24' font-family='sans-serif'%3EТвоё фото%3C/text%3E%3C/svg%3E";
</script>

<template>
	<main class="no-result">
		<h1 class="no-result__title">Нам очень грустно!:( Не обижай нас:(</h1>
		<div class="no-result__photo">
			<img
				src="/sad.png"
				alt="photo"
				class="no-result__img"
				@error="($event.target as HTMLImageElement).src = placeholder"
			/>
		</div>
		<button class="no-result__back" @click="router.push('/')">
			Изменить свое решение
		</button>
	</main>
</template>

<style scoped>
.no-result {
	min-height: 100vh;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	padding: 2rem;
	text-align: center;
}

.no-result__title {
	font-size: 1.5rem;
	color: #1a1a1a;
	margin-bottom: 1.5rem;
}

.no-result__photo {
	max-width: 400px;
	border-radius: 0.5rem;
	overflow: hidden;
	margin-bottom: 1.5rem;
	box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
}

.no-result__img {
	width: 100%;
	height: auto;
	display: block;
	object-fit: cover;
}

.no-result__back {
	padding: 0.5rem 1.5rem;
	border-radius: 0.25rem;
	border: 1px solid #1a1a1a;
	background: #fff;
	color: #1a1a1a;
	font-size: 1rem;
	cursor: pointer;
	transition:
		background 0.2s,
		color 0.2s;
}
.no-result__back:hover {
	background: rgb(245, 141, 242);
	color: #fff;
}
</style>
