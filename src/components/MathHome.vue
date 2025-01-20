<script setup>
import { computed, onMounted, onUpdated , ref, watch, createApp} from 'vue';

createApp(()=> {
  setMathProblem();
})
// onMounted(() => {
//   generateTwoRandomNums();
// });
let msg = 'Shall we play a game?'
let userInput = '';
let randomMax = 99; // allow user set of this? or by radio buttons??
let randomMax2 = 9;
var showProblem = false;
let random1 = ref(0);
var random2 = ref(0);
let gameStarted = ref(false);
let corrIncorrMsg = ref('');
let correct = ref(false);
let error = ref(false);
let showFeedBack = ref(false);
//let userInput = ref('');

watch(random1, (newR1, oldR1) => {
  console.log('old random1 = ' + oldR1 + '; new random1 = ' + newR1 );
});
watch(random2, (newR2, oldR2) => {
  console.log('old random2 = ' + oldR2 + '; new random2 = ' + newR2 );
});

const setProblemText = computed(() => {
  return gameStarted.value ? 'Want a new math problem?' : 'Click Here to Start!';
});

const feedbackMsg = computed(() => {
  return `Your answer was: ${ userInput }. ${ corrIncorrMsg.value }`;
});

const correctState = computed(() => {
  return correct.value;
});

const errorState = computed(() => {
  return error.value;
});

const hasFeedback = computed(() => {
  return showFeedBack.value;
});

function generateTwoRandomNums(){
  let r1, r2;
  r1 = Math.floor(Math.random() * randomMax);
  r2 = Math.floor(Math.random() * randomMax2);
  console.log(`R1 is ${r1} and R2 is ${r2}!!`)
  return [r1,r2];
}

function checkAnswer() {
  console.log('userInput = '+ userInput);
  console.log(`Random1 is ${random1.value} and Random2 is ${random2.value}!!`)
  let corrAnswer = random1.value * random2.value;
  let msg = 'The correct answer is : ' + corrAnswer + '.';
  let isCorrect = (Number(userInput) === corrAnswer);
  showFeedBack.value = true;
  if(isCorrect) {
    correct.value = true;
    error.value = false;
  } else {
    correct.value = false;
    error.value = true;
  }
  corrIncorrMsg.value = isCorrect ? ' Great job!' : " I'm sorry--pls try again...";
  //alert(' Your answer was: ' + userInput + corrIncorrMsg.value);
}
function setMathProblem() {
  resetAll()
  if(!gameStarted.value) {
    gameStarted.value = true;
    console.log('GAME ON!');
  }
  const nums = generateTwoRandomNums();
  random1.value = nums[0];
  random2.value = nums[1];
  userInput = '';
  console.log(`Random1 is ${nums[0]} and Random2 is ${nums[1]}!!`)
}

function resetAll() {
  corrIncorrMsg.value = '';
  correct.value = false;
  error.value = false;
  showFeedBack.value = false;
}
//computed 

//features: hint/help button
// clear input and reset problem on correct
</script>

<template>
    <div class="border-div">
        Header Text--Math Practice
        <div class="row centeredRow">
          <h1>{{ msg }}</h1>
          <button class="btn btn-primary w-50" @click="setMathProblem()"> Want a new math problem? </button>

        </div>
        <br>
        <div class="card w-100 ms-3 me-3">
          <div class="row centeredRow">
              <p class="ms3">What is {{ random1 }} x {{ random2 }}?</p>
          </div>
          <!-- <div class="ms-3">You've chosen to attempt the problem! Showproblem? {{ showProblem }}</div> -->
          <input v-model="userInput" class="form-control w-25 ms-3 centeredInput" @keyup.enter="checkAnswer()" type="number">
          <br>
          <div class="row centeredRow">
            <button class="btn btn-secondary w-50 mb-2" @click="checkAnswer()"> Check Answer! </button>
          </div>
        </div>
        <div v-if="hasFeedback" class="card w-100 ms-3 me-3 mt-3 feedbackCard" :class="[ {correct : correctState}, {error : errorState} ]">
          <div class="row centeredRow" >
              <p class="ms3">{{ feedbackMsg }}</p>
          </div>
        </div>
    </div>
</template>
<style lang="css">
.border-div {
  box-sizing: border-box; 
  border: 5px;
}
.centeredRow {
  justify-content: center;
}
.centeredInput {
  align-self: center;
}
p.ms3, h1 {
    text-align: center;
}
.feedbackCard {
  &.correct {
    background-color: aquamarine;
  }
  &.error {
    background-color: coral;
  }
}
</style>
