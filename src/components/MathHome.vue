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
let corrIncorrMsg = '';

watch(random1, (newR1, oldR1) => {
  console.log('old random1 = ' + oldR1 + '; new random1 = ' + newR1 );
});
watch(random2, (newR2, oldR2) => {
  console.log('old random2 = ' + oldR2 + '; new random2 = ' + newR2 );
});

const setProblemText = computed(() => {
  return gameStarted.value ? 'Want a new math problem?' : 'Click Here to Start!';
});

function generateTwoRandomNums(){
  let r1, r2;
  r1 = Math.floor(Math.random() * randomMax);
  r2 = Math.floor(Math.random() * randomMax2);
  console.log(`R1 is ${r1} and R2 is ${r2}!!`)
  return [r1,r2];
}

function popAnswerMultiply() {
  //userInput = document.getElementById('userProvidedAnswer').value;
  console.log('userInput = '+ userInput);
  console.log(`Random1 is ${random1.value} and Random2 is ${random2.value}!!`)
  let corrAnswer = random1.value * random2.value;
  let msg = 'The correct answer is : ' + corrAnswer + '.';
  let isCorrect = (Number(userInput) === corrAnswer);
  corrIncorrMsg = isCorrect ? ' Great job!' : " I'm sorry--pls try again...";
  alert(' Your answer was: ' + userInput + corrIncorrMsg);
}
function setMathProblem() {
  if(!gameStarted.value) {
    gameStarted.value = true;
    console.log('GAME ON!');
  }
  const nums = generateTwoRandomNums();
  random1.value = nums[0];
  random2.value = nums[1];
  // showProblem = true;

  console.log(`Random1 is ${nums[0]} and Random2 is ${nums[1]}!!`)
  // let corrAnswer = nums[0] * nums[1];
  // let msg = 'The correct answer is : ' + corrAnswer + '.';
  // let isCorrect = (Number(userInput) === corrAnswer);
  // let corrIncorrMsg = isCorrect ? ' Great job!' : " I'm sorry--pls try again...";
  // alert(msg + ' Your answer was: ' + Number(userInput) + corrIncorrMsg);
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
          <button class="btn btn-primary w-50" @click="setMathProblem()"> {{ setProblemText }} </button>

        </div>
        <br>
        <div class="card w-100 ms-3 me-3">
          <div class="row centeredRow">
              <p class="ms3">What is {{ random1 }} x {{ random2 }}?</p>
          </div>
          <!-- <div class="ms-3">You've chosen to attempt the problem! Showproblem? {{ showProblem }}</div> -->
          <input v-model.number="userInput" class="form-control w-25 ms-3 centeredInput" @keyup.enter="popAnswerMultiply()" type="number" v-model="userInput">
          <br>
          <div class="row centeredRow">
            <button class="btn btn-secondary w-50 mb-2" @click="popAnswerMultiply()"> Check Answer! </button>
            <!-- <div class="col-12"> -->
            <!-- </div> -->
          </div>
        </div>

        <!-- <div class="input-group mb-3">
          <input id="userProvidedAnswer" @keyup.enter="popAnswerMultiply()" type="number" class="form-control" placeholder="Your answer here . . . " aria-label="your answer" aria-describedby="basic-addon2">
          <span @click="popAnswerMultiply()"  class="input-group-text" id="basic-addon2">Check Answer!</span>
        </div> -->
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
</style>
