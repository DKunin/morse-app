<template>
  <div id="app">
      <div>
        <h3>Morse Translator</h3>
        <textarea v-model="message" placeholder="add multiple lines"></textarea>
      </div>
      <div class="translated">
        {{ translated }}
      </div>
      <div>
        <button type="button" @click="handleTranslate">-/.-./.-/-./.../.-../.-/-/.</button>
      </div>
  </div>
</template>

<script>
import xmorse from 'xmorse';

const pause = 70;
const dotDuration = 150;
const dashDuration = dotDuration * 3;

export default {
  name: 'app',
  data: () => ({
    message: '',
    translated: '',
  }),
  methods: {
    handleTranslate() {
      const translated = xmorse.encode(this.message);
      const vibrations = translated.split('').reduce((newArray, singleNode) => {
        if (singleNode === '.') {
          return newArray.concat([dotDuration, pause]);
        }
        if (singleNode === '-') {
          return newArray.concat([dashDuration, pause]);
        }
        if (singleNode === '/') {
          return newArray.concat([0, dotDuration]);
        }
        return newArray;
      }, []);
      window.navigator.vibrate(vibrations);
      this.$set(this, 'translated', translated);
      // eslint-disable-next-line
      console.log(translated, vibrations);
    },
  },
};
</script>

<style>
body, html, #app {
  margin: 0;
  padding: 0;
  font-size: 25px;
  height: 100%;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: space-between;
}

#app > * {
  padding: 0 20px;
  font-size: 1em;
}

textarea {
  width: 100%;
  min-height: 70px;
}

button {
  width: 100%;
  font-size: 1.3em;
  margin-bottom: 50px;
}
.translated {
  margin: 20px;
  border: 1px solid rgba(0, 0, 0, .07);
  background-color: rgba(0, 0, 0, .07);
}

h3 {
  font-size: 1.4em;
  text-align: center;
}

</style>
