<template>
  <q-page class="flex column" style="max-width: 800px; margin: auto">
    <h1 class="text-h3 text-center">
      Guess The Number <small>Between 1 to 20</small>
    </h1>
    <div class="flex justify-center items-center q-my-lg">
      <hr class="number-bg" />
      <q-input
        readonly
        borderless
        v-model="randomNumber"
        class="randomnumber"
      />
    </div>
    <div class="row q-pt-lg q-col-gutter-xl">
      <div class="col-6">
        <q-form
          @submit="checkNumber"
          @reset="reset"
          class="flex column items-center"
        >
          <q-input
            outlined
            v-model.number="number"
            type="number"
            placeholder="eg - 1"
            class="number"
          />
          <div class="q-gutter-md q-mt-lg">
            <q-btn
              label="Check"
              type="submit"
              color="primary"
              :disable="this.score < 1"
            />
            <q-btn label="Reset" type="reset" color="secondary" />
          </div>
        </q-form>
      </div>
      <div class="col-6">
        <h2 class="text-h4 status q-mt-none">🚀 Let's Start Game</h2>
        <div class="text-h4 flex no-wrap">
          🏅 Score:
          <q-input
            readonly
            borderless
            v-model="score"
            dense
            class="text-h4 q-pa-none q-ml-sm"
            style="width: 150px"
          />
        </div>
        <h2 class="text-h4">🎯 HighScore : <span class="highscore">0</span></h2>
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";
import { Notify } from "quasar";

export default defineComponent({
  name: "PageIndex",
  setup() {
    return {
      randomNumber: ref(""),
      number: ref(""),
      score: ref(20),
      highscore: ref(0),
    };
  },
  created() {
    this.randomNumber = Math.trunc(Math.random() * 20 + 1);
  },
  methods: {
    checkNumber() {
      if (this.number == null || this.number == "") {
        Notify.create("Please Enter Number");
      } else if (this.number < this.randomNumber) {
        // Notify.create("Too Low");
        document.querySelector(".status").textContent = "Too Low";
        --this.score;
        if (this.score < 1) {
          // Notify.create("Game Over");
          document.querySelector(".status").textContent = "😟 Game Over";
          document.querySelector("body").style.backgroundColor = "#ffc3c3";
        }
        this.number = "";
      } else if (this.number > this.randomNumber) {
        // Notify.create("Too High");
        document.querySelector(".status").textContent = "Too High";
        --this.score;
        if (this.score < 1) {
          // Notify.create("Game Over");
          document.querySelector(".status").textContent = "😟 Game Over";
          document.querySelector("body").style.backgroundColor = "#ffc3c3";
        }
        this.number = "";
      } else if (this.number === this.randomNumber) {
        // Notify.create("You Win");
        document.querySelector(".status").textContent = "🏆 You won the game";
        document.querySelector("body").style.backgroundColor = "#c4ffc4";
        if (this.score > this.highscore) {
          this.highscore = this.score;
          document.querySelector(".highscore").textContent = this.highscore;
        }
        document.querySelector(".randomnumber").style.filter = "blur(0)";
      }
    },
    reset() {
      document.querySelector(".status").textContent = "🚀 Let's Start Game";
      this.randomNumber = Math.trunc(Math.random() * 20 + 1);
      this.number = "";
      this.score = 20;
      document.querySelector("body").style.backgroundColor = "#fff";
      document.querySelector(".randomnumber").style.filter = "blur(10px)";
    },
  },
});
</script>
<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");
@import url("https://fonts.googleapis.com/css2?family=New+Rocker&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Aladin&display=swap");
body {
  // font-family: "Press Start 2P", cursive;
  // font-family: "New Rocker", cursive;
  font-family: "Aladin", cursive;
}
.randomnumber {
  width: 150px;
  height: 150px;
  filter: blur(8px);
  input {
    width: 150px;
    height: 150px;
    text-align: center;
    font-size: 50px;
    color: #fff;
    font-weight: bold;
    background-color: grey;
  }
}
.number-bg {
  position: absolute;
  width: 100%;
  border-width: 5px;
}
.number {
  width: 166px;
  height: 80px;
  .q-field__control,
  input {
    width: 166px;
    height: 80px;
    font-size: 40px;
  }
}
</style>
