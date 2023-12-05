<script setup>
import { computed, onMounted } from 'vue';

onMounted(() => {
  generateTwoRandomNums();
});
let msg = 'Hi there world!!'
let userInput = '';
let randomMax = 9; // allow user set of this? or by radio buttons??

let r1,r2, random1, random2;

// computed(()=> {
//   //return Math.random()
//   return Math.floor(Math.random() * randomMax);
// } )
//let randomNum2 = 3;

random1 = computed(() => {
  return r1;
});
random2 = computed(() => {
  return r2;
});
function generateTwoRandomNums(){
  let r1, r2;
  r1 = Math.floor(Math.random() * randomMax);
  r2 = Math.floor(Math.random() * randomMax);
  console.log(`R1 is ${r1} and R2 is ${r2}!!`)
}

function popAnswerMultiply() {
  let corrAnswer = this.r1 * this.r2;
  let msg = 'The correct answer is : ' + corrAnswer + '.';
  let isCorrect = (Number(userInput) === corrAnswer);
  let corrIncorrMsg = isCorrect ? ' Great job!' : " I'm sorry--pls try again...";
  alert(msg + ' Your answer was: ' + Number(userInput) + corrIncorrMsg);
  // if(Number(answerInput.value) === randomNum1 * randomNum2){
  //   alert('you are correct!');
  // } else {
  //   alert('oops--pls try again');
  // }
   

}
//computed 
</script>

<template>
    <div>
        Header Text--Math Practice
        <h1>{{ msg }}</h1>
        <!-- <button @click="$emit('click', alertTest)">click for action</button> -->
        <!-- <button v-on:click.stop="$emit('click', $event)">OK</button> -->
        <div><p>What is {{ random1 }} x {{ random2 }}?</p></div>
        <button @click="popAnswerMultiply()"> Check Answer! </button>
        <input type="text" v-model="userInput">
    </div>
</template>


<!-- <script>

export default {
    //props: ['text'],
    emits: ['alertTest']
  }
</script> -->
<!-- export default {
    props: ['text'],
    emits: ['accepted']
  } -->