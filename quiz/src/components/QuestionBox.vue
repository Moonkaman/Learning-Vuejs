<template>
  <div>
    <b-jumbotron>
      <template slot="lead">{{question.question}}</template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >{{ answer }}</b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >Submit</b-button>
      <b-button @click="next" variant="success">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    question: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: false
    };
  },
  computed: {
    answers() {
      let answers = [
        ...this.question.incorrect_answers,
        this.question.correct_answer
      ];
      return answers;
    }
  },
  watch: {
    question: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.question.incorrect_answers,
        this.question.correct_answer
      ];

      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.question.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;

      this.increment(isCorrect);
    },
    answerClass(i) {
      let answerClass = "";

      if (!this.answered && this.selectedIndex === i) {
        answerClass = "selected";
      } else if (this.answered && this.correctIndex === i) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === i &&
        this.correctIndex !== i
      ) {
        answerClass = "wrong";
      }

      return answerClass;
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 20px;
}

.list-group-item {
  transition: background 0.2s ease;
}

.list-group-item:hover {
  background: #eeeeee;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background: rgb(61, 169, 241);
}

.correct {
  background: rgb(126, 207, 126);
}

.wrong {
  background: rgb(228, 150, 150);
}
</style>


