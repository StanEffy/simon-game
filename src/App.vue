<template>
  <div id="app">
    <h1>Funniest game ever</h1>
    <p>Current level is {{ level }}</p>
    <div class="container">
      <div class="inputs-cover">
        <input
          type="text"
          data-number="0"
          readonly
          class="input-color input-color--red"
          @click="
            ($event) => {
              playSound($event);
              checkAnswer($event);
            }
          "
        />
        <input
          type="text"
          data-number="1"
          readonly
          class="input-color input-color--yellow"
          @click="
            ($event) => {
              playSound($event);
              checkAnswer($event);
            }
          "
        />
        <input
          type="text"
          data-number="2"
          readonly
          class="input-color input-color--blue"
          @click="
            ($event) => {
              playSound($event);
              checkAnswer($event);
            }
          "
        />
        <input
          type="text"
          data-number="3"
          readonly
          class="input-color input-color--green"
          @click="
            ($event) => {
              playSound($event);
              checkAnswer($event);
            }
          "
        />
      </div>
      <audio src="" class="hidden">
        <source class="audio audio-0" src="./assets/sounds/0.mp3" />
        <source class="audio audio-1" src="./assets/sounds/1.mp3" />
        <source class="audio audio-2" src="./assets/sounds/2.mp3" />
        <source class="audio audio-3" src="./assets/sounds/3.mp3" />
      </audio>
      <div class="controllers-container">
        <button type="button" class="button--start" v-on:click="startGame">
          START!
        </button>
        <ul class="levels-list">
          <li>
            <input
              type="radio"
              class="level"
              id="easy"
              name="level"
              checked
              @click="delayConst = 1500"
            />
            <label for="easy">Easy</label>
          </li>
          <li>
            <input
              type="radio"
              class="level"
              name="level"
              id="medium"
              @click="delayConst = 1000"
            />
            <label for="medium">Medium</label>
          </li>
          <li>
            <input
              type="radio"
              class="level"
              name="level"
              id="nightmare"
              @click="delayConst = 400"
            />
            <label for="nightmare">Nightmare</label>
          </li>
        </ul>
        <div>
          <p v-if="mistake" class="lost announcement">
            You have lost. Let's give it another try!
          </p>
          <p v-if="nextLevel" class="next-level announcement">
            Let's go to the next level
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  methods: {
    getRandomInt() {
      return Math.round(Math.random() * 3);
    },
    constructLevel() {
      this.sounds = [];
      for (let i = 0; i < this.level; i++) {
        this.sounds.push(this.getRandomInt());
      }
    },
    getColor() {
      const color = document.querySelectorAll(".input-color");
      return color;
    },
    makeSound(delay, elem) {
      setTimeout(() => {
        this.colors[elem].classList.add("input-color--opacity");
        let audio = new Audio(this.soundsArr[elem].src);
        audio.play();
        setTimeout(
          () => this.colors[elem].classList.remove("input-color--opacity"),
          250
        );
      }, delay);
    },
    checkAnswer() {
      if (this.game) {
        //let currentSound = this.sounds.shift();
        if (event.target.dataset.number == this.sounds.shift()) {
          if (this.sounds.length == 0) {
            this.nextLevel = true;
            this.level += 1;
            setTimeout(() => this.startGame(), this.delayConst);
          }
        } else {
          this.mistake = true;
          this.game = false;
          this.level = 1;
        }
      } else console.log("you should start a game");
    },
    getSrc() {
      const sounds = document.querySelectorAll(".audio");
      return sounds;
    },
    playSound: function(event) {
      const sounds = document.querySelectorAll(".audio");
      let audio = new Audio(sounds[event.target.dataset.number].src);
      audio.play();
    },
    startGame() {
      this.nextLevel = false;
      this.mistake = false;
      this.game = true;
      this.colors = this.getColor();

      this.soundsArr = this.getSrc();

      this.constructLevel();
      let delay = 0;
      for (let i = 0; i < this.sounds.length; i++) {
        this.makeSound(delay, this.sounds[i]);
        delay += this.delayConst;
      }
    },
  },
  data() {
    return {
      level: 1,
      soundsArr: [],
      sounds: [],
      delayConst: 1500,
      colors: [],
      game: false,
      mistake: false,
      nextLevel: false,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  margin: 0 auto;
  display: flex;
  width: 700px;
}
.inputs-cover {
  display: flex;
  flex-wrap: wrap;
  width: 370px;
  flex-direction: row;
  margin-right: 40px;
}
.input-color {
  cursor: pointer;
  width: 150px;
  height: 150px;
}
.input-color:active {
  opacity: 0.7;
}
.input-color--opacity {
  opacity: 0.7;
}
.input-color--red {
  background-color: #ff0000;
}
.input-color--yellow {
  background-color: #d9ff00;
}
.input-color--blue {
  background-color: #2600ff;
}
.input-color--green {
  background-color: #13a100;
}
.levels-list {
  text-align: left;
  list-style: none;
}
.controllers-container {
  width: 300px;
  display: flex;
  flex-direction: column;
}
.announcement {
  font-size: 26px;
}
.lost {
  color: #ff0000;
}
.next-level {
  color: #13a100;
}
.hidden {
  display: none;
}
</style>
