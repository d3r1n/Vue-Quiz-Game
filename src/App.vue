<template>

	<div class="main-content" v-if="questions != null">

		<div class="score">

			<span id="correct">
				Doğru: {{ correct }}
			</span>

			<span id="wrong">
				Yanlış: {{ wrong }}
			</span>

		</div>
	
		<Question :question="questions[question]" @state="handleState"/>

		<div class="buttons-container">

			<button class="btn btn-prev" @click="prevQuestion">
				<span class="btn-front">Önceki</span>
			</button>

			<button class="btn btn-next" @click="nextQuestion">
				<span class="btn-front">Sonraki</span>
			</button>

		</div>

	</div>

</template>

<script>
import Question from './components/Question.vue'

export default {
	data() {
		return {
			question: 0,
			questions: null,
			correct: 0,
			wrong: 0,
		}
	},
	components: {
		Question
	},
	async beforeMount() {
		this.questions = await fetch('/questions.json').then(response => response.json()).then(data => data.questions)
	},
	methods: {
		nextQuestion() {
			if (this.question < this.questions.length - 1) {
				this.resetAnswerClasses()
				this.applyAnimation()
				this.question++
			}
		},
		prevQuestion() {
			if (this.question != 0) {
				this.resetAnswerClasses()
				this.applyAnimation()
				this.question--
			}
		},
		resetAnswerClasses() {
			let answers = document.getElementsByClassName('question-answer');
			for (let i = 0; i < answers.length; i++) {
				answers[i].classList.remove('correct')
				answers[i].classList.remove('wrong')
			}
		},
		applyAnimation() {
			document.getElementsByClassName('question')[0].classList.add('scale-in-center')
		},
		handleState(state) {
			if (state == 'correct') {
				this.correct++
				// play a random audio from array
				let audio_arr = ["/amogus.mp3", "/lessgo.mp3"]
				let audio = new Audio(audio_arr[Math.floor(Math.random() * audio_arr.length)])
				audio.play()
			} else {
				this.wrong++
				let audio = new Audio("/boom.mp3")
				audio.play()
			}
		}
	}
}
</script>