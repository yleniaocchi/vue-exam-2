<script setup>
import { ref, computed } from 'vue'

import img1 from './assets/img1.png'
import img2 from './assets/img2.png'
import img3 from './assets/img3.png'

const questions = ref([
	{
		question: 'In quale film i vendicatori si uniscono per la prima volta?',
		answer: 0,
		options: [
			'The Avengers',
			'Thor: Ragnarok',
			'Avengers: Age of Ultron'
		],
		selected: null
	},
	{
		question: 'Quale gemma è nascosta a Vormir?',
		answer: 2,
		options: [
			'Tempo',
			'Spazio',
			'Anima'
		],
		selected: null
	},
	{
		question: 'Quale personaggio interpreta il soldato invernale?',
		answer: 1,
		options: [
			'Clint Barton',
			'Bucky Barnes',
			'Sam Wilson'
		],
		selected: null
	},

	{
		question: 'Quale Avenger è in grado di sollevare Mjolnir oltre a Thor?',
		answer: 1,
		options: [
			'Capitan America',
			'Visione',
			'Hulk'
		],
		selected: null
	},

	{
		question: 'In "Guardiani della galassia" cosa cerca Ronan?',
		answer: 2,
		options: [
			'Tesseract',
			'Occhio di Agamotto',
			'Orb'
		],
		selected: null
	},

	{
		question: 'Chi è il villain principale in Spider-Man: Homecoming?',
		answer: 0,
		options: [
			'Vulture',
			'Mysterio',
			'Green Goblin'
		],
		selected: null
	}

])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected != null && q.answer == q.selected) {
			console.log('correct');
			value++
		}
	})
	return value
})

const resultImage = computed(() => {
	if (score.value == 1) {
		return img1
	} else if (score.value < questions.value.length) {
		return img2
	} else {
		return img3
	}
})

const getCurrentQuestion = computed(() => {
	let question = questions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})

const SetAnswer = (e) => {
	questions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}

const NextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}

	quizCompleted.value = true
}
</script>

<template>
	<main class="app">
		<h1> MARVEL QUIZ</h1>

		<section class="quiz" v-if="!quizCompleted">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score"> Punteggio {{ score }}/{{ questions.length }}</span>
			</div>

			<div class="options">
				<label v-for="(option, index) in getCurrentQuestion.options" :for="'option' + index" :class="`option ${getCurrentQuestion.selected == index
			? index == getCurrentQuestion.answer
				? 'correct'
				: 'wrong'
			: ''
			} ${getCurrentQuestion.selected != null &&
				index != getCurrentQuestion.selected
				? 'disabled'
				: ''
			}`">
					<input type="radio" :id="'option' + index" :name="getCurrentQuestion.index" :value="index"
						v-model="getCurrentQuestion.selected" :disabled="getCurrentQuestion.selected"
						@change="SetAnswer" />
					<span>{{ option }}</span>
				</label>
			</div>

			<button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
				{{
			getCurrentQuestion.index == questions.length - 1
				? 'The end'
				: getCurrentQuestion.selected == null
					? 'Scegli una risposta'
					: 'Prossima domanda'
		}}
			</button>
		</section>

		<section v-else>
			<h2>Hai finito il quiz!</h2>
			<img :src="resultImage" alt="Risultato del quiz" class="result-image" />

			<p v-if="score == 1">Non sai niente!</p>
			<p v-else-if="score < questions.length / 2">Potevi fare di meglio!</p>
			<p v-else-if="score < questions.length">Buon lavoro!</p>
			<p v-else>Sei un vero esperto Marvel!</p>

			<p>Il tuo punteggio è {{ score }}/{{ questions.length }}</p>
		</section>
	</main>
</template>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Montserrat', sans-serif;
}

body {
	background-image: url(BACKGORUND.jpg);
	background-size: cover;
	color: #FFF;
}

.app {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 2rem;
	height: 100vh;
	width: 100vw;
	padding-right: 12rem;
}

h1 {
	font-size: 2rem;
	margin-bottom: 2rem;
}

.quiz {
	border-radius: 1.2rem;
	background-color: #000000;
	padding: 1rem;
	width: 100%;
	max-width: 800px;
	min-width: 800px;
}

.quiz-info {
	display: flex;
	justify-content: space-between;
	margin-bottom: 1rem;
}

.quiz-info .question {
	color: #8F8F8F;
	font-size: 1.25rem;
}

.quiz-info.score {
	color: #FFF;
	font-size: 1.25rem;
}

.options {
	margin-bottom: 1rem;
}

.option {
	padding: 1rem;
	display: block;
	background-color: #1f1b1b;
	margin-bottom: 0.5rem;
	border-radius: 0.5rem;
	cursor: pointer;
}

.option:hover {
	background-color: #646464;
}

.option.correct {
	background-color: #2cce7d;
}

.option.wrong {
	background-color: #ff5a5f;
}

.option:last-of-type {
	margin-bottom: 0;
}

.option.disabled {
	opacity: 0.5;
}

.option input {
	display: none;
}

button {
	appearance: none;
	outline: none;
	border: none;
	cursor: pointer;
	padding: 0.5rem 1rem;
	background-color: #cd0000;
	color: #000000;
	font-weight: 700;
	text-transform: uppercase;
	font-size: 1.2rem;
	border-radius: 0.5rem;
}

button:disabled {
	opacity: 0.5;
}

h1 {
	font-family: "Oswald", sans-serif;
	font-weight: bolder;
	font-size: 50px;
}


h2 {
	font-size: 2rem;
	margin-bottom: 2rem;
	text-align: center;
}

p {
	color: #8F8F8F;
	font-size: 1.5rem;
	text-align: center;
}

.result-image {
	z-index: 999999;
	max-width: 300px;
	height: auto;
	margin-bottom: 1rem;
	align-items: center;
}
</style>