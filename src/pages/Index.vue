<template>
  <q-page class="page-container">
    <!-- GIVE INPUT -->

    <div class="startContainer" v-if="giveInput">
      <InputComponent :text="'Enter your question'" v-model="question" />

      <br />

      <InputComponent :text="'Enter first answer'" v-model="answerOne" />
      <InputComponent :text="'Enter second answer'" v-model="answerTwo" />
      <InputComponent :text="'Enter third answer'" v-model="answerThree" />
      <InputComponent :text="'Enter fourth answer'" v-model="answerFour" />

      <br />

      <select
        class="correct-answer-input"
        name="wichAnswerIsCorrect"
        v-model="correctAnswer"
        required
      >
        <option value="" disabled selected hidden>
          Which answer is correct:
        </option>
        <option value="first">First</option>
        <option value="second">Second</option>
        <option value="third">Third</option>
        <option value="fourth">Fourth</option>
      </select>

      <br />

      <button class="save-answer" @click="pushAnswers()">Save answer</button>

      <button class="start-button" @click="startQuizClicked()">
        Start Quiz
      </button>
    </div>

    <!-- START OF QUIZ -->

    <div v-if="startQuiz">
      <div class="center-question">
        <div class="question">
          {{ selectedQuestion.vraag }}
        </div>
      </div>

      <div class="answer-component-container">
        <div class="answer-container">
          <AnswerComponent
            v-for="(antwoord, index) in selectedQuestion.antwoorden"
            :key="index"
            :answer="antwoord.answer"
            :isCorrect="givenCorrectAnswers[index]"
            :isIncorrect="givenIncorrectAnswers[index]"
            @click="answerCheck(antwoord.isTrue, index)"
          />
        </div>
      </div>
      <div class="next-button-container">
        <button class="next-button" @click="nextQuestion()">next</button>
      </div>
    </div>

    <!-- THE END -->

    <div v-if="isQuizFinished">
      <h2 class="result-text">Your score:</h2>

      <h4 class="positive" v-if="succeeded > 0.5">
        {{ amountOfCorrectAnswers }} / {{ amountAnsweredQuestions }}
      </h4>
      <h4 class="negative" v-else>
        {{ amountOfCorrectAnswers }} / {{ amountAnsweredQuestions }}
      </h4>
    </div>
  </q-page>
</template>

<script lang="ts">
import { defineComponent, ref, Ref, computed } from 'vue';
import AnswerComponent from 'components/AnswerComponent.vue';
import InputComponent from 'src/components/InputComponent.vue';

export default defineComponent({
  name: 'indexPage',
  components: {
    AnswerComponent,
    InputComponent,
  },

  setup() {
    const startQuiz: Ref<boolean> = ref(false);
    const giveInput: Ref<boolean> = ref(true);
    const isQuizFinished = computed(() => {
      return !startQuiz.value && !giveInput.value;
    });

    const question: Ref<string> = ref('');
    const answerOne: Ref<string> = ref('');
    const answerTwo: Ref<string> = ref('');
    const answerThree: Ref<string> = ref('');
    const answerFour: Ref<string> = ref('');

    const correctAnswer: Ref<string> = ref('');
    const amountAnsweredQuestions: Ref<number> = ref(0);
    const succeeded: Ref<number> = ref(0);

    // Function om quiz te starten
    function startQuizClicked() {
      startQuiz.value = true;
      giveInput.value = false;
    }

    const quizObjects = [
      {
        vraag: 'This is a test question',
        antwoorden: [
          { answer: 'Alright', isTrue: false },
          { answer: 'Okay', isTrue: false },
          { answer: 'Fine', isTrue: true },
          { answer: 'Good', isTrue: false },
        ],
      },
    ];

    // Function om q&a's toe te voegen vanuit user input
    function pushAnswers() {
      if (
        answerOne.value === '' ||
        answerTwo.value === '' ||
        answerThree.value === '' ||
        answerFour.value === '' ||
        question.value === '' ||
        correctAnswer.value === ''
      ) {
        window.alert('Fill everything out!');
      } else {
        let aOneResult = false;
        let aTwoResult = false;
        let aThreeResult = false;
        let aFourResult = false;

        switch (correctAnswer.value) {
          case 'first':
            aOneResult = true;
            break;
          case 'second':
            aTwoResult = true;
            break;
          case 'third':
            aThreeResult = true;
            break;
          case 'fourth':
            aFourResult = true;
            break;
        }

        quizObjects.push({
          vraag: question.value,
          antwoorden: [
            { answer: answerOne.value, isTrue: aOneResult },
            { answer: answerTwo.value, isTrue: aTwoResult },
            { answer: answerThree.value, isTrue: aThreeResult },
            { answer: answerFour.value, isTrue: aFourResult },
          ],
        });

        question.value = '';
        answerOne.value = '';
        answerTwo.value = '';
        answerThree.value = '';
        answerFour.value = '';
        correctAnswer.value = '';
      }
    }

    let i = 0;
    const selectedQuestion = ref(quizObjects[i]);

    // Function om naar de volgende q&a te gaan
    function nextQuestion() {
      i++;
      selectedQuestion.value = quizObjects[i];
      givenCorrectAnswers.value = [];
      givenIncorrectAnswers.value = [];
      if (i === quizObjects.length) {
        startQuiz.value = false;
        amountAnsweredQuestions.value = quizObjects.length - 1;
        succeeded.value =
          amountOfCorrectAnswers.value / amountAnsweredQuestions.value;
      }
    }

    const givenCorrectAnswers: Ref<Array<boolean>> = ref([]);
    const givenIncorrectAnswers: Ref<Array<boolean>> = ref([]);

    let amountOfCorrectAnswers = ref(0);

    // Function om te kijken of het antwoord juist is als er geklikt wordt
    function answerCheck(isCorrect: boolean, indexNumber: number) {
      if (isCorrect === true) {
        givenCorrectAnswers.value[indexNumber] = true;
        if (i !== 0) {
          amountOfCorrectAnswers.value++;
        }
      }
      if (isCorrect === false) {
        givenIncorrectAnswers.value[indexNumber] = true;
      }
    }

    return {
      startQuiz,
      giveInput,
      startQuizClicked,
      givenCorrectAnswers,
      givenIncorrectAnswers,
      answerCheck,
      quizObjects,
      selectedQuestion,
      nextQuestion,
      pushAnswers,
      question,
      answerOne,
      answerTwo,
      answerThree,
      answerFour,
      correctAnswer,
      amountOfCorrectAnswers,
      isQuizFinished,
      amountAnsweredQuestions,
      succeeded,
    };
  },
});
</script>

<style lang="scss" scoped>
.page-container {
  margin: 40px 30px 5px 30px;
  display: flex;
  justify-content: center;
}

.correct-answer-input {
  border: 0.05rem solid #ff8c00;
  color: deeppink;
  width: 18rem;
  height: 2rem;
  text-align: center;

  &:focus {
    outline: none !important;
    border: 0.1rem solid #ff8c00;
    box-shadow: 0 0 0.3rem #ff1493;
  }
}

.save-answer {
  display: block;
  margin: 5px;
  background-image: linear-gradient(to right bottom, #fdaa44, #fd53ae);
  border: 0.05rem solid #e98d1c;
  cursor: pointer;
}

.start-button {
  height: 4rem;
  width: 15rem;
  background-image: radial-gradient(circle, #008793, #00bf72, #a8eb12);
  font-size: 2.5rem;
  margin-top: 4rem;
  border-radius: 2rem;
  cursor: pointer;
}

.center-question {
  display: flex;
  justify-content: center;
  margin-bottom: 25px;
}
.question {
  color: rgb(255, 20, 94);
  font-size: 27px;
  justify-content: center;
}

.answer-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.startContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.next-button-container {
  display: flex;
  justify-content: center;
}

.next-button {
  width: 7rem;
  height: 2.5rem;
  background-image: radial-gradient(circle, #008793, #00bf72, #a8eb12);
  border-color: #54caeb;
  border-radius: 2rem;
  margin-right: 1rem;
  margin-top: 2rem;
}

.result-text {
  color: deeppink;
}

.positive {
  color: green;
  margin-left: 7rem;
}

.negative {
  color: red;
  margin-left: 7rem;
}

@media only screen and (max-width: 768px) {
  .answer-component-container {
    width: 50%;
    margin-left: 14.5rem;
  }
}
</style>
