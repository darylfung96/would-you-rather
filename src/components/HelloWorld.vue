<template>
  <div>
    <div :style="bg_image"></div>

    <div class="center-items">
      <transition name="fade" mode="out-in">
        <a :key="currentQuestion" v-if="shuffledQuestions.length">{{
          shuffledQuestions[currentQuestion]
        }}</a>
      </transition>
    </div>
    <a @click="changeIndex(false)" class="previous">Previous</a>
    <a @click="changeIndex(true)" class="next">Next</a>
  </div>
</template>

<script>
import QUESTIONS from "../questions";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  methods: {
    shuffleQuestions: (a) => {
      for (let i = a.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [a[i], a[j]] = [a[j], a[i]];
      }
      return a;
    },
    changeIndex: function (value) {
      if (value) this.currentQuestion++;
      else this.currentQuestion--;

      if (this.currentQuestion < 0) this.currentQuestion = 0;
      else if (this.currentQuestion > this.shuffledQuestions.length)
        this.currentQuestion = this.shuffledQuestions.length;
    },
  },
  created: function () {
    this.shuffledQuestions = this.shuffleQuestions(QUESTIONS);
  },
  data() {
    return {
      bg_image: {
        backgroundImage: `url(${require("../../public/background-image.jpg")})`,
        height: "100vh",
        width: "100%",
        filter: "brightness(50%)",
        position: "absolute",
      },
      shuffledQuestions: [],
      currentQuestion: 0,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
a {
  color: #fff;
  font-size: 5vh;
  position: relative;
  text-align: center;
  margin-bottom: 10vh;
  margin-left: 5vh;
  margin-right: 5vh;
}
.center-items {
  height: 100vh;
  width: 100%;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.previous {
  position: absolute;
  display: block;
  bottom: 0%;
  left: 10vh;
  cursor: pointer;
}
.previous:hover {
  transform: scale(1.2);
}

.next {
  position: absolute;
  display: block;
  bottom: 0%;
  right: 10vh;
  cursor: pointer;
}
.next:hover {
  transform: scale(1.2);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
