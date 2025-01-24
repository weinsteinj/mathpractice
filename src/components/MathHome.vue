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
let randomMax = 9; // allow user set of this? or by radio buttons??
let randomMax2 = 9;
let random1 = ref(0);
var random2 = ref(0);
let streakNumRef = ref(0);
let scoreRef = ref(0);
let difficultyWeightRef = ref(1);
let gameStarted = ref(false);
let corrIncorrMsg = ref('');
let correct = ref(false);
let error = ref(false);
let showFeedBack = ref(false);
let isCheckable = ref(true);
//let userInput = ref('');

watch(random1, (newR1, oldR1) => {
  console.log('old random1 = ' + oldR1 + '; new random1 = ' + newR1 );
});
watch(random2, (newR2, oldR2) => {
  console.log('old random2 = ' + oldR2 + '; new random2 = ' + newR2 );
});

onMounted(() => setMathProblem())

// const setProblemText = computed(() => {
//   return gameStarted.value ? 'Want a new math problem?' : 'Click Here to Start!';
// });

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

const enableCheckButton = computed(() => {
  return isCheckable.value;
});

const getScore = computed(() => {
  return scoreRef.value;
});

const getStreakNum = computed(() => {
  return streakNumRef.value;
});

function generateTwoRandomNums(n = 55, n2 = 9){ // change to take the number of digits desired
  //let r1, r2;
  let result = [];
  result.push(generateRandomNum(n));
  result.push(generateRandomNum(n2));
  // r1 = Math.floor(Math.random() * n);
  // r2 = Math.floor(Math.random() * n2);
  console.log(`R1 is ${result[0]} and R2 is ${result[1]}!!`)
  return result;
}

function generateRandomNum(n) {
  let q;
  q = Math.floor(Math.random() * n);
  if(n > 9 && q < 10) {
    return generateRandomNum(n);
  } else {
    return q;
  }
}

function setRandomMax(n,n2){
  randomMax = n;
  randomMax2 = n2;
  difficultyWeightRef.value = n.toString().length + n2.toString().length;
  console.log('diff wght = '+ difficultyWeightRef.value);
  setMathProblem();
}

function checkAnswer() {
  console.log('userInput = '+ userInput);
  console.log(`Random1 is ${random1.value} and Random2 is ${random2.value}!!`)
  let corrAnswer = random1.value * random2.value;
  //let msg = 'The correct answer is : ' + corrAnswer + '.';
  let isCorrect = (Number(userInput) === corrAnswer);
  showFeedBack.value = true;
  if(isCorrect) {
    correct.value = true;
    error.value = false;
    streakNumRef.value++;
    console.log(`streakNumRef.value = ${streakNumRef.value}`);
    scoreRef.value = scoreRef.value + (5 * difficultyWeightRef.value );
    isCheckable.value = false;
  } else {
    correct.value = false;
    error.value = true;
    streakNumRef.value = 0;
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
  const nums = generateTwoRandomNums(randomMax, randomMax2);
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
  isCheckable.value = true;
}
//computed 

//features: hint/help button
// clear input and reset problem on correct
</script>

<template>
    <div class="border-div">
        <div class="row centeredRow">
          <div class="col-6 text-start">Score: {{ getScore }}</div>
          <div class="col-6 text-end">Streak: {{ getStreakNum }}</div>
        </div>
        <div class="row centeredRow">
          <h1>{{ msg }}</h1>
          <button class="btn btn-primary w-50" @click="setMathProblem()"> Want a new math problem? </button>
        </div>
        <div class="row centeredRow">
          Select Game Mode:
        </div>
        <div class="row centeredRow">
          <div class="col-3">
            <button class="btn btn-primary w-50" @click="setRandomMax(9,9)"> 1 x 1 </button>
          </div>
          <div class="col-3">
            <button class="btn btn-primary w-50" @click="setRandomMax(9,55)"> 1 x 2 </button>
          </div>
          <div class="col-3">
            <button class="btn btn-primary w-50" @click="setRandomMax(55,55)"> 2 x 2 </button>
          </div>
          <div class="col-3">
            <button class="btn btn-primary w-50" @click="setRandomMax(55,9)"> 2 x 1 </button>
          </div>
        </div>

        <br>
        <div class="card w-100 answerCard">
          <div class="row centeredRow">
              <p class="ms3">What is {{ random1 }} x {{ random2 }}?</p>
          </div>
          <input v-model="userInput" class="form-control w-25 ms-3 centeredInput" @keyup.enter="checkAnswer()" type="number">
          <br>
          <div class="row centeredRow">
            <button class="btn btn-secondary w-50 mb-2" @click="checkAnswer()" :inert="!enableCheckButton"> Check Answer! </button>
          </div>
        </div>
        <div v-if="hasFeedback" class="card w-100 feedbackCard" :class="[ {correct : correctState}, {error : errorState} ]">
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
  height: 2rem;
  &.correct {
    background-color: aquamarine;
  }
  &.error {
    background-color: coral;
  }
}
.answerCard {
  height: calc(100%-2rem);
}
</style>
