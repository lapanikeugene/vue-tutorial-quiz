<script setup lang="ts">
  import {ref, computed} from "vue";
  const questions = ref([
    {question:"What is Bahnhof?", 
    answer:0, 
    options:["Railway station","supermarket","pupil","school"],
    selected:null},
    {question:"What is Hof?", 
    answer:2, 
    options:["Railway station","City","yard","street"],
    selected:null},
    {question:"What is einverstanden?", 
    answer:0, 
    options:["Agree","understand","disagree","get 1 book for"],
    selected:null},
    {question:"What is Gehalt?", 
    answer:1, 
    options:["Document","Salary","Pasport","Notebook"],
    selected:null},
  ])
  const quizComplited = ref();
  const currentQuestion = ref(0);
  const score = computed(()=>{
    let value =  0;
    questions.value.map(q=>{
      if(q.answer === q.selected){
        value++;
      }
    
    })
    return value;
})

const getCurrentQuestion = computed(()=>{
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;


})

const SetAnswer = (e) => {
  questions.value[currentQuestion.value].selected = e.target.value;
  e.target.value = null;
  
}

const NextQuestion = ()=>{
  if(currentQuestion.value >= questions.value.length-1){ quizComplited.value = true; return;}
  currentQuestion.value++;
  
}

</script>

<template>
   <main class="app">
      <h1>The Quiz</h1>
      <section class="quiz">
        <div class="quiz-info">
          <span class="question">{{getCurrentQuestion.question}}</span>
          <span class="score">Score {{score}} / {{questions.length}}</span>
        </div>

        <div class="options">
          <label v-for="(item, index) in getCurrentQuestion.options" 
          :for="'option' + index" 
          :key="'option' + index" 
          :class="`item ${
            getCurrentQuestion.selected === index ? 
            index === getCurrentQuestion.answer ? 
            'correct':
            'wrong'
            :
            ''}   
            ${getCurrentQuestion.selected != null &&
            index !== getCurrentQuestion.selected ? 
              'disabled' : 
              ''}
              `"
          
          >
          <input type="radio" :name="getCurrentQuestion.index"
          :value="index"
          v-model="getCurrentQuestion.selected"
          :disabled="getCurrentQuestion.selected"

          @click="SetAnswer"
         />
         <span>{{item}}</span>
          </label>

        </div>
      </section>
   </main>
</template>

<style scoped>
*{
  margin:0;
  padding: 0;
  box-sizing: border-box;
}
body{
  background-color: #456b7c;
  color: white;
}
</style>
