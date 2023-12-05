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
var showProblem =   false;
let random1 = ref(0);
var random2 = ref(0);

watch(random1, (newR1, oldR1) => {
  console.log('old random1 = ' + oldR1 + '; new random1 = ' + newR1 );
});
watch(random2, (newR2, oldR2) => {
  console.log('old random2 = ' + oldR2 + '; new random2 = ' + newR2 );
});



function generateTwoRandomNums(){
  let r1, r2;
  r1 = Math.floor(Math.random() * randomMax);
  r2 = Math.floor(Math.random() * randomMax);
  console.log(`R1 is ${r1} and R2 is ${r2}!!`)
  return [r1,r2];
}

function popAnswerMultiply() {
  console.log(`Random1 is ${random1.value} and Random2 is ${random2.value}!!`)
  let corrAnswer = this.random1 * this.random2;
  let msg = 'The correct answer is : ' + corrAnswer + '.';
  let isCorrect = (Number(userInput) === corrAnswer);
  let corrIncorrMsg = isCorrect ? ' Great job!' : " I'm sorry--pls try again...";
  alert(msg + ' Your answer was: ' + Number(userInput) + corrIncorrMsg);
}
function setMathProblem() {
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
</script>

<template>
    <div>
        Header Text--Math Practice
        <h1>{{ msg }}</h1>
        <button @click="setMathProblem()"> Click Here to Start! </button>
        <div><p>What is {{ random1 }} x {{ random2 }}?</p></div>
        <div>You've chosen to attempt the problem! Showproblem? {{ showProblem }}</div>
        <button @click="popAnswerMultiply()"> Check Answer! </button>
        <input type="text" v-model="userInput">
    </div>
</template>
