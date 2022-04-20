<template>

	<div class="question">

		<div class="question-content" v-if="question.question != '-'">
			<p>{{ question.question }}</p>
		</div>

		<img :src="question.picture" v-if="question.picture != '-'" />

		<div class="question-answers">
			<div class="question-answer" v-for="(answer, index) in question.answers" @click="checkAnswer(index); applyAnimation(index)" :key="index" :id="'answer-' + index">
				{{ answer }}
			</div>
		</div>

	</div>

</template>

<script>
export default {
	props: {
		question: {
			type: Object,
			required: true
		}
	},
	methods: {
		checkAnswer(index) {
			if (this.question.correct == index) {
				document.getElementById('answer-' + index).classList.toggle('correct');
				this.$emit('state', 'correct');
			} else {
				document.getElementById('answer-' + index).classList.toggle('wrong');
				document.getElementById('answer-' + this.question.correct).classList.toggle('correct');
				this.$emit('state', 'wrong');
			}
		},
		applyAnimation(index) {
			let answer = document.getElementById('answer-' + index);
			answer.classList.add('jello-horizontal')
			setTimeout(() => {
				answer.classList.remove('jello-horizontal');
			}, 900);
		}
	},
	watch: {
		question: () => {
			setTimeout(() => {
				document.getElementsByClassName('question')[0].classList.remove('scale-in-center');
			}, 1010);
		}
	}
}
</script>