<script setup>
import { ref, computed } from 'vue'

import img1 from './assets/img1.png'
import img2 from './assets/img2.jpg'
import img3 from './assets/img3.jpg'

const questions = ref([
	{
		question: 'Qual è il colore della scuderia Ferrari?',
		answer: 0,
		options: [
			'Rosso',
			'Giallo',
			'Blu'
		],
		selected: null
	},
	{
		question: 'Quale pilota ha vinto il suo primo campionato nel 2010?',
		answer: 2,
		options: [
			'Lewis Hamilton',
			'Felipe Massa',
			'Sebastian Vettel'
		],
		selected: null
	},
	{
		question: 'Qual è stata la prima squadra di Michael Schumacher in F1?',
		answer: 1,
		options: [
			'Benetton',
			'Jordan',
			'Ferrari'
		],
		selected: null
	},

	{
		question: 'Quale scuderia ha vinto il primo campionato nel 1950?',
		answer: 1,
		options: [
			'Ferrari',
			'Alfa Romeo',
			'Williams'
		],
		selected: null
	},

	{
		question: 'In che anno la Ferrari ha vinto il campionato costruttori per l ultima volta?',
		answer: 2,
		options: [
			'2006',
			'2007',
			'2008'
		],
		selected: null
	},

	{
		question: 'Quale team ha portato Max Verstappen al debutto in F1?',
		answer: 0,
		options: [
			'Toro Rosso',
			'Redbull',
			'Mclaren'
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
		<h1> FORMULA 1 QUIZ</h1>

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

			<p v-if="score == 1"> GOATIFI: non sai nulla!</p>
			<p v-else-if="score < questions.length / 2">Potevi fare di meglio! Copy that!</p>
			<p v-else-if="score < questions.length">Good job! Copy!</p>
			<p v-else> Il predestinato vince il gran premio d'Italia!</p>

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
	background-image: url(BACKGROUND.jpg);
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
	color: #b3b3b3;
	font-size: 1.5rem;
	text-align: center;
}

.result-image {
	z-index: 999999;
	max-width: 300px;
	max-height: 200px;
	margin-bottom: 1rem;
	margin-left: 30px;
	align-items: center center;
}


@media (max-width: 768px) {
    .app {
        padding: 1rem;
        height: auto;
        width: 100%;
        padding-right: 0;
    }

    .quiz {
        width: 100%;
        min-width: auto;
        max-width: 100%;
        padding: 0.5rem;
    }

    .quiz-info {
        flex-direction: column;
        align-items: center;
        text-align: center;
    }

	body {
	background-image: url(backgroundphone.jpg);
	background-size: cover;
	color: #FFF;
	height: 100vh;
	overflow: hidden;
    }

    h1 {
        font-size: 1rem;
    }

    h2 {
        font-size: 1rem;
    }

    p {
        font-size: 0.8rem;
    }

    .option {
        font-size: 0.8rem;
        padding: 0.8rem;
    }

    button {
        font-size: 1rem;
        padding: 0.5rem 1rem;
    }

    .result-image {
        max-width: 50%;
        max-height: 50%;
		margin-left: 100px;
    }
}

</style>