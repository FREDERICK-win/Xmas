<template>
  <div id="app">
    <img v-if="!showPresent || (canOpenPresent && showPresent)" class="santa" src="./assets/santa.png" />
    <div v-if="canOpenPresent && showPresent">
      <video controls autoplay>
        <source src="https://frederick.win/upload/xmas/video.mp4" type="video/mp4" />
        Get a proper browser, motherfucker.
      </video>

      <h1>Merry Christmas faggot</h1>
    </div>

    <div v-if="showPresent && !canOpenPresent">
      <h1>Shalom Mr. Verleih!</h1>
      <p>{{ timeUntilXmas }}</p>
      <img class="verleih" src="./assets/verleih.png" />
    </div>

    <div v-if="!showPresent" class="form__field">
      <input
        type="text"
        class="form__input"
        v-model="key"
        :placeholder="placeholder"
        @keyup="checkPassword"
        @focus="handlePlaceholder()"
        @blur="handlePlaceholder()"
      />
      <p>Don't answer before Xmas 6pm</p>

      <img class="we" src="./assets/we.png" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      showPresent: false,
      key: '',
      placeholder: 'Who is your best friend?',
      validKeys: ['freddy', 'frederick'],
      canOpenPresent: false,
      now: new Date(),
    };
  },
  computed: {
    timeUntilXmas() {
      // eslint-disable-next-line global-require
      const humanizeDuration = require('humanize-duration');
      const currentMonth = this.now.getMonth() + 1;
      const currentDay = this.now.getDate();
      let nextChristmasYear = this.now.getFullYear();
      if (currentMonth === 12 && currentDay > 25) nextChristmasYear += 1;
      const nextChristmasDate = `${nextChristmasYear}-12-24T17:00:00.000Z`;
      const christmasDay = new Date(nextChristmasDate);
      return `Come back in ${humanizeDuration(Math.floor(christmasDay.getTime() - this.now.getTime()), {
        conjunction: ' and ', serialComma: false, maxDecimalPoints: 0,
      })}`;
    },
  },
  methods: {
    checkPassword() {
      if (this.validKeys.includes(this.key.toLowerCase())) this.showPresent = true;
    },
    handlePlaceholder() {
      const input = document.getElementsByClassName('form__input')[0];
      if (input.placeholder === this.placeholder) input.setAttribute('placeholder', '');
      else input.setAttribute('placeholder', this.placeholder);
    },
  },
  mounted() {
    const currentMonth = this.now.getMonth() + 1;
    const currentDay = this.now.getDate();
    let nextChristmasYear = this.now.getFullYear();
    if (currentMonth === 12 && currentDay > 25) nextChristmasYear += 1;
    const nextChristmasDate = `${nextChristmasYear}-12-24T17:00:00.000Z`;
    const christmasDay = new Date(nextChristmasDate);
    setInterval(() => {
      if (christmasDay.getTime() < this.now.getTime()) this.canOpenPresent = true;
    }, 1000);
  },
  created() {
    const self = this;
    setInterval(() => {
      self.now = new Date();
    }, 1000);
  },
};
</script>

<style>
:root {
  background-color: #a0a0a0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

p {
  font-family: 'Caveat', cursive;
  font-size: 1.5em;
  color: black;
}

.verleih {
  position: fixed;
  left: 50%;
  bottom: 0;
  transform: translate(-50%, 0);
  margin: 0 auto;
  max-height: 70vh;
  max-width: 80vw;
}

.santa {
  max-width: 40vw;
  max-height: 30vh;
}

.we {
  position: fixed;
  bottom: 0;
  right: 0;
  width: 25vw;
}

h1 {
  font-size: 5em;
  font-family: 'Caveat', cursive;
  color: black;
  margin-bottom: 0;
}

video {
  width: 40vw;
  margin-top: -4px;
}

input {
  font: inherit;
  outline: 0;
}

.form__field {
  position: relative;
  margin-top: -4px;
}

.form__input {
  border-radius: 0.3em;
  border-style: solid;
  border-width: 2px;
  font-size: 3rem;
  padding: 0.5em 1em;
  text-align: center;
}
</style>
