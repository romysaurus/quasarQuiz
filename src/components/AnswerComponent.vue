<template>
  <button
    class="answers standard"
    :class="{ right: isCorrect, wrong: isIncorrect }"
  >
    {{ answer }}
  </button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'AnswerComponent',
  props: {
    answer: {
      type: String,
      required: true,
    },
    isCorrect: {
      type: Boolean,
    },
    isIncorrect: {
      type: Boolean,
    },
  },
});
</script>

<style lang="scss" scoped>
.answers {
  margin: 20px 100px;
  height: 50px;
  width: 500px;
  cursor: pointer;
}

.standard {
  background-color: #ff8c0025;
  border: 0.05rem solid #ff8c00;
  color: deeppink;
}

.right {
  background-color: rgba(156, 223, 156, 0.98);
  border-color: rgb(24, 174, 24, 0.694);
  color: rgb(81, 39, 61);
  font-size: 15px;
  font-weight: bold;
}

.wrong {
  background-color: rgba(226, 150, 150, 0.98);
  border-color: rgba(255, 0, 0, 0.694);
  color: rgb(81, 39, 61);
  font-weight: bold;
}
</style>
