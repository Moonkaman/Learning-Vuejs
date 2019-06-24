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
          :class="[selectedIndex === index ? 'selected' : '']"
        >{{ answer }}</b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    question: Object,
    next: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: []
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


