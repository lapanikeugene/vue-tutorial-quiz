<script setup lang="ts">
  import {ref, computed} from "vue";
  type questionType = {
    index:number,
    options:string[],
    answer:number,
    selected:number|null,
    question:string,

  }
  const questions = ref<questionType[]>([
    {
      index:0,
      question:"What is Bahnhof?", 
    answer:0, 
    options:["Railway station","supermarket","pupil","school"],
    selected:null},
    {question:"What is Hof?", 
    index:1,
    answer:2, 
    options:["Railway station","City","yard","street"],
    selected:null},
    {question:"What is einverstanden?", 
    answer:0,
    index:2, 
    options:["Agree","understand","disagree","get 1 book for"],
    selected:null},
    {question:"What is Gehalt?", 
    answer:1,
    index:3, 
    options:["Document","Salary","Pasport","Notebook"],
    selected:null},
  ])
  const quizComplited = ref();
  const currentQuestion = ref(0);
  const score = computed(()=>{
    let value =  0;
    questions.value.map(q=>{
      if (q.selected != null && q.answer == q.selected) {
			console.log('correct');
			value++
		}
    
    })
    return value;
})

const getCurrentQuestion = computed(()=>{
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;


})

const SetAnswer = (e: Event) => {
  if(e.target === null) return;
  const el = e.target as HTMLInputElement
  console.log(el.value);
  questions.value[currentQuestion.value].selected = Number(el.value);
  console.log(questions.value);
  el.value = "";
  
}

const NextQuestion = ()=>{
  if(currentQuestion.value >= questions.value.length-1){ quizComplited.value = true; return;}
  currentQuestion.value++;
  
}

</script>


<template>
	<main class="app">
		<h1>The Quiz</h1>
		
		<section class="quiz" v-if="!quizComplited">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score">Score {{ score }}/{{ questions.length }}</span>
			</div>
			
			<div class="options">
				<label 
					v-for="(option, index) in getCurrentQuestion.options" 
					:for="'option' + index" 
          :key="'option' + index"
					:class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer 
								? 'correct' 
								: 'wrong'
							: ''
					} ${
						getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
							? 'disabled'
							: ''
					}`">
					<input 
						type="radio" 
						:id="'option' + index" 
						:name="String(getCurrentQuestion.index || '0')" 
						:value="index" 
						v-model="getCurrentQuestion.selected" 
						:disabled="getCurrentQuestion.selected!=null || false"
						@change="SetAnswer" 
					/>
					<span>{{ option }}</span>
				</label>
			</div>
			
			<button 
				@click="NextQuestion" 
				:disabled="getCurrentQuestion.selected===null">
				{{ 
					getCurrentQuestion.index == questions.length - 1 
						? 'Finish' 
						: getCurrentQuestion.selected == null
							? 'Select an option'
							: 'Next question'
				}}
			</button>
		</section>

		<section v-else>
			<h2>You have finished the quiz!</h2>
			<p>Your score is {{ score }}/{{ questions.length }}</p>
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
	background-color: #271c36;
	color: #FFF;
}

.app {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 2rem;
	height: 100vh;
}

h1 {
	font-size: 2rem;
	margin-bottom: 2rem;
}

.quiz {
	background-color: #382a4b;
	padding: 1rem;
	width: 100%;
	max-width: 640px;
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
	background-color: #271c36;
	margin-bottom: 0.5rem;
	border-radius: 0.5rem;
	cursor: pointer;
}

.option:hover {
	background-color: #2d213f;
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
	background-color: #2cce7d;
	color: #2d213f;
	font-weight: 700;
	text-transform: uppercase;
	font-size: 1.2rem;
	border-radius: 0.5rem;
}

button:disabled {
	opacity: 0.5;
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
</style>