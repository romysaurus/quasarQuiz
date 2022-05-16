<template>
  <q-page id="pageContainer">
    <!-- GIVE INPUT -->

    <div class="startContainer" v-if="giveInput">
      <InputComponent
        :text="'Enter your question'"
        :valueName="question"
        @inputChange="question = $event"
      />

      <br />

      <input
        class="inputField"
        type="text"
        placeholder="Enter first answer"
        v-model="answerOne"
      />

      <input
        class="inputField"
        type="text"
        placeholder="Enter second answer"
        v-model="answerTwo"
      />

      <input
        class="inputField"
        type="text"
        placeholder="Enter third answer"
        v-model="answerThree"
      />

      <input
        class="inputField"
        type="text"
        placeholder="Enter fourth answer"
        v-model="answerFour"
      />

      <br />

      <label for="wichAnswerCorrect">Wich answer is correct:</label>
      <select id="wichCorrect" name="wichAnswerCorrect" v-model="correctAnswer">
        <option value="first">First</option>
        <option value="second">Second</option>
        <option value="third">Third</option>
        <option value="fourth">Fourth</option>
      </select>

      <br />

      <button id="saveAnswer" @click="pushAnswer()">Save answer</button>

      <button id="startButton" @click="startQuizClicked()">Start Quiz</button>
    </div>

    <!-- START OF QUIZ -->

    <div v-if="startQuiz">
      <div id="centerQuestion">
        <div class="question">
          {{ selectedQuestion.vraag }}
        </div>
      </div>

      <div id="componentContainer">
        <div id="answerContainer">
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

      <button id="nextButton" @click="nextQuestion()">next</button>
    </div>

    <!-- THE END -->

    <div v-if="isQuizFinished">
      <h2>Your score:</h2>

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
import InputComponent from 'components/InputComponent.vue';

export default defineComponent({
  name: 'indexPage',
  components: {
    AnswerComponent,
    InputComponent,
  },

  setup() {
    const startQuiz = ref(false);
    const giveInput = ref(true);
    const isQuizFinished = computed(() => {
      return !startQuiz.value && !giveInput.value;
    });
    const question = ref('');
    const answerOne = ref('');
    const answerTwo = ref('');
    const answerThree = ref('');
    const answerFour = ref('');
    const aOneBoolean = ref('');
    const aTwoBoolean = ref('');
    const aThreeBoolean = ref('');
    const aFourBoolean = ref('');

    const correctAnswer = ref('');
    const amountAnsweredQuestions = ref(0);
    const succeeded = ref(0);

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
    function pushAnswer() {
      const questionValue = question.value;
      const answerOneValue = answerOne.value;
      const answerTwoValue = answerTwo.value;
      const answerThreeValue = answerThree.value;
      const answerFourValue = answerFour.value;

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
        vraag: questionValue,
        antwoorden: [
          { answer: answerOneValue, isTrue: aOneResult },
          { answer: answerTwoValue, isTrue: aTwoResult },
          { answer: answerThreeValue, isTrue: aThreeResult },
          { answer: answerFourValue, isTrue: aFourResult },
        ],
      });

      question.value = '';
      answerOne.value = '';
      answerTwo.value = '';
      answerThree.value = '';
      answerFour.value = '';
      correctAnswer.value = '';
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
      pushAnswer,
      question,
      answerOne,
      answerTwo,
      answerThree,
      answerFour,
      aOneBoolean,
      aTwoBoolean,
      aThreeBoolean,
      aFourBoolean,
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
#pageContainer {
  margin: 5px 30px;
  display: flex;
  justify-content: center;
}

#startButton {
  height: 4rem;
  width: 15rem;
  background-image: radial-gradient(circle, #aabbd8, #bfe370);
  font-size: 2.5rem;
  margin-top: 2rem;
  border-radius: 2rem;
  cursor: pointer;
}

#centerQuestion {
  display: flex;
  justify-content: center;
  margin-bottom: 25px;
}
.question {
  color: rgb(1, 59, 59);
  font-size: 27px;
  justify-content: center;
}

#answerContainer {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.startContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
}

#saveAnswer {
  display: block;
  margin: 5px;
}

select {
  margin-left: 4px;
}

#nextButton {
  width: 4.5rem;
  height: 2.5rem;
  background-image: radial-gradient(circle, #c3e184bc, #30aaccea);
  border-color: #54caeb;
  border-radius: 2rem;
  margin-right: 1rem;
  margin-top: 1rem;
  float: right;
}

.inputField {
  margin: 5px;
  display: block;
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
  #componentContainer {
    width: 50%;
    height: 100%;
    margin-left: 14.5rem;
  }

  #nextButton {
    margin-top: -19rem;
    margin-right: 19rem;
  }
}
</style>
